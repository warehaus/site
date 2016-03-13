---
layout: docs-page
title: Raw Objects
id: objects-api
---

The lowest layer in the Warehaus API allows for direct object access by an object-ID. This page explains Warehaus's object model and the relevant APIs.

# Objects in Warehaus

Every object in Warehaus is stored in the database. For example, we could have 10 servers and 2 clusters. This would result in 12 objects being stored in the database. Also, each object would have its own attributes, for example a server should have attributes describing its CPUs and network interfaces, while a cluster could have an owner attribute.

Aside from the per-object attributes, Warehaus also needs to store attributes that would tell it what each object is and where it belongs. Those are the **type** and **parent** attributes.

So, each object should at least contain the following attributes:

* `id`: a unique UUID for each object in the database, across all labs
* `slug`: an API name that allows for querying the object through API and is unique in the object's lab
* `type_id`: the `id` of this object's type object
* `parent_id`: the `id` of this object's parent object

In the next two section we'll describe the use of `type_id` and `parent_id`.

## Parent Objects

The `parent_id` attribute implies some hierarchy between objects. However we can store whatever parent/child relation we want in the database, the Warehaus API enforces some rules that are relevant for hardware objects.

### Labs are objects too
 
The first rule is that **labs are objects**.

Warehaus does that because labs act as the highest-level container of objects, so labs shouldn't share things between them. This limits in some ways, but really simplifies the API since no two objects can have the same `slug` and `parent_id`.

As a result, labs are the only objects that have a `parent_id` of `null`.

### Objects in labs

Labs are the top-level objects, so one lab's children are all the objects in that specific lab.

This sounds simple, but this is another important rule. For example, say that we have a lab with 1 cluster object and 2 server objects, and say that we want to mark that the 2 servers belong to the cluster. Should we change the servers' `parent_id` attribute? According to our rules the answer is *no*.

The reason we never change an object's `parent_id` is that the parent/child relation is somewhat physical. Think of it as the relation between a data-center and a server &ndash; they don't change very often.

Frequent changes in the `parent_id` attribute of objects might fail since they would break the previous rule of never having two `slug`s as children of the same parent. As we'll see later, it also changes API URLs which is another downside to changing an object's `parent_id` attribute.

To allocate servers to clusters, or mark any other relationship, we add additional attributes. In the cluster-server example, we add a `cluster_id` attribute to server objects.

## Type Objects

As mentioned earlier, each object points to a **type object** using the `type_id` attribute. The type object describes the object's behavior and multiple objects can point to the same type object.

But what are type objects and what describes their behavior?

Well, type objects are regular objects with one additional attribute: `type_key`. This attribute is a string that selects the type of the type object, also called the **type class**.

Type classes are fixed per Warehaus installation, so type objects essentially create a 3-level type system.

### Type object hierarchy

Much like regular objects, type object also have a hierarchy. This is useful so that we can address type objects just like regular objects through the API. We'll see examples of that in the next section.

To allow this hierarchy, for every lab we also create a type object for that lab. Every type object in the lab is a child of the lab's type object. This allows for type objects to be bound to a lab, as well as the regular objects.

### Illustration

In our previous example of 1 lab, 1 cluster and 2 servers, the full picture of the objects and their relationships is:

![Objects Example](/assets/img/objects-example.png)

In this example we can see the lab and its type objects. The cluster is a child of the lab, has a type object of its own and the cluster's type object is a child of the lab's type object. The similar structure exists for the two servers.

# Objects API

As mentioned above, the lowst API layer allows for direct object access.

This kind of API is great when we know an object ID and want to fetch it directly, but not very convenient when we want to work with objects by their slug. In the next section we'll see how to call objects by name/slug.

## `/api/v1/hardware/objects`

This API returns all objects in the database. The objects are returned under one top-level `objects` attribute. In the future this API would support pagination so additional attributes such as `page_size` and `total_results` should be added.

Example output:

    {
       "objects":[
          {
             "id":"d3a03a2d-8c2c-47b2-922a-8f1c037f9aed",
             "parent_id":null,
             "display_name":"Kim",
             "slug":"kim",
             "type_id":"8ffc25b1-7c7b-43ed-b92d-f388f470b7e6"
          },
          {
             "id":"8ffc25b1-7c7b-43ed-b92d-f388f470b7e6",
             "parent_id":null,
             "type_key":"builtin-lab",
             "slug":null,
             "type_id":null
          },
          {
             "status":"success",
             "errors":null,
             "display_name":"debby1",
             "type_id":"453cbe85-c8b9-4695-be5f-4abfaf748d6d",
             "id":"60ee3a51-aa58-4573-a4fa-905723a0097c",
             "parent_id":"d3a03a2d-8c2c-47b2-922a-8f1c037f9aed",
             "block_devices":"{u'major': u'8', u'bytes': u'8589934592', u'readonly': u'0', u'removable': u'0', u'device': u'sda', u'type': u'disk', u'minor': u'0'}",
             "pci_devices":"{u'vendor': u'Intel Corporation', u'type': u'Host bridge', u'name': u'440FX - 82441FX PMC [Natoma]', u'address': u'00:00.0'}",
             "cluster_id":"ded74bd7-7791-48c0-a7c1-27001da4d291",
             "net":"{u'eth1': {u'mac': u'08:00:27:92:c9:12', u'inet6': [u'fe80::a00:27ff:fe92:c912/64'], u'inet': [u'192.168.99.101/24']}, u'eth0': {u'mac': u'08:00:27:97:24:93', u'inet6': [u'fe80::a00:27ff:fe97:2493/64'], u'inet': [u'10.0.2.15/24']}}",
             "last_heartbeat":"2016-02-13 12:29:47.145000+00:00",
             "slug":"debby1"
          },
          {
             "display_name":{
                "plural":"servers",
                "singular":"server"
             },
             "type_id":null,
             "slug":"server",
             "parent_id":"8ffc25b1-7c7b-43ed-b92d-f388f470b7e6",
             "type_key":"builtin-server",
             "id":"453cbe85-c8b9-4695-be5f-4abfaf748d6d"
          }
       ]
    }

In this example we can:

* One lab called `kim`
* The lab's type object
* One server called `debby1`
* The server's type object, a child of the lab's type object

## `/api/v1/hardware/objects/<obj_id>`

Returns one object by its ID. For example `/api/v1/hardware/objects/453cbe85-c8b9-4695-be5f-4abfaf748d6d`:

    {
       "display_name":{
          "plural":"servers",
          "singular":"server"
       },
       "type_id":null,
       "slug":"server",
       "parent_id":"8ffc25b1-7c7b-43ed-b92d-f388f470b7e6",
       "type_key":"builtin-server",
       "id":"453cbe85-c8b9-4695-be5f-4abfaf748d6d"
    }

## `/api/v1/hardware/types`

Returns all type-classes this Warehaus instance supports. For example:

    {
       "types":[
          {
             "display_name":"Lab",
             "type_key":"builtin-lab"
          },
          {
             "display_name":"Server",
             "type_key":"builtin-server"
          },
          {
             "display_name":"Cluster",
             "type_key":"builtin-cluster"
          }
       ]
    }

# What's next?

In the next section we'll see the hierarchical labs API which allows for accessing objects by their `slug`s and hierarchy.
