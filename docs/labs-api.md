---
layout: docs-page
title: Labs API
id: labs-api
---

As mentioned in before, labs are the top-level containers in Warehaus. This means that every object has to be contained in some lab.

This page describes how to query and operate on objects based on their names alone.

# Slug hierarchy

All objects have a slug, including labs. In general, labs and their objects are organized under `/api/v1/labs` such that every path component is the object's slug.

For example, the object `debby1` in the `kim` lab is represented by the API prefix of:

    /api/v1/labs/kim/debby1/

# Objects and actions

Note that the trailing `/` from the last example is important. API paths are generally organized as:

    /api/v1/labs/<lab-name>/<child1>/.../<childN>/action

This means that when addressing a certain object we first specify its lab, then the first object in the lab, then the next child and so on, until we get to the object we want to address. Unlike **raw objects' API**, here we must address an object by addressing all objects that lead to it in the object hierarchy.

The `action` is what we want the object to do. We can omit `action`, which addresses the object directly. An empty action usually means getting the object, but the behavior depends on the type object and may vary.

    # No action, address a specific object
    /api/v1/labs/<lab-name>/<child1>/.../<childN>/

    # Invoke a specific action on an object
    /api/v1/labs/<lab-name>/<child1>/.../<childN>/action

Also, note that since this is a RESTful API an action has different meanings for each HTTP verb &ndash; `GET`, `POST`, `PUT` or `DELETE`.

For example, you can update a server's cluster by invoking a `PUT` request on the `cluster` action:

    curl -X PUT -H "Content-Type: application/json" -d '{"cluster_id": "..."}' http://example.com/api/v1/labs/kim/debby1/cluster

# Accessing type objects

As described in the previous page ([Raw Objects](objects.md)), every object has a type object.

Type objects determine the object's behavior, but they're also useful for queries such as getting all objects of a certain type.

A type object is accessed by addressing the `~` slug of a certain object. For example:

    /api/v1/labs/kim/~/

addresses the type object of the `kim` lab.

Like regular objects, type objects have actions too. For example, `POST`ing on a cluster's type object creates a new cluster of that type.

Most actions are type-specific, however all type objects have two useful actions:

### Getting all objects of type

To get all objects of a certain type, simply invoke the `objects` action of that type. For example:

    /api/v1/labs/kim/~/server/objects

returns all objects of type `server` in the `kim` lab. This works because as mentioned in the previous page, type objects in labs are children of the lab's type object. Therefore, type objects have a slug and we can address them from their lab's type object. The result in our case would be

    {
       "objects":[
          {
             "status":"success",
             "errors":null,
             "display_name":"debby1",
             "type_id":"96b0032d-90c2-461f-b1b8-423a2d948d1c",
             "id":"b5fd4f63-0afc-4ed2-9346-dda0ac6acf20",
             "parent_id":"d3a03a2d-8c2c-47b2-922a-8f1c037f9aed",
             "block_devices":"{u'major': u'8', u'bytes': u'8589934592', u'readonly': u'0', u'removable': u'0', u'device': u'sda', u'type': u'disk', u'minor': u'0'}",
             "pci_devices":"{u'vendor': u'Intel Corporation', u'type': u'Host bridge', u'name': u'440FX - 82441FX PMC [Natoma]', u'address': u'00:00.0'}",
             "net":"{u'eth1': {u'mac': u'08:00:27:92:c9:12', u'inet6': [u'fe80::a00:27ff:fe92:c912/64'], u'inet': [u'192.168.99.101/24']}, u'eth0': {u'mac': u'08:00:27:97:24:93', u'inet6': [u'fe80::a00:27ff:fe97:2493/64'], u'inet': [u'10.0.2.15/24']}}",
             "last_heartbeat":"2016-02-14 08:27:07.235000+00:00",
             "slug":"debby1"
          }
       ]
    }

In the same way, we can get all objects that are like another object. For example, if we have a server named `debby1` in our lab, we can get all objects that share the same type object as it without knowing the slug of the type object. This call would return the same output as the previous one:

    /api/v1/labs/kim/debby1/~/objects

### Getting all children of a type object

Type objects support querying their children, which are type objects as well. For example, we could query all type objects of a lab by querying the children of the lab's type object:

    /api/v1/labs/kim/~/children

Which results in:

    {
       "children":[
          {
             "display_name":{
                "plural":"servers",
                "singular":"server"
             },
             "type_id":null,
             "slug":"server",
             "parent_id":"8ffc25b1-7c7b-43ed-b92d-f388f470b7e6",
             "type_key":"builtin-server",
             "id":"96b0032d-90c2-461f-b1b8-423a2d948d1c"
          }
       ]
    }

# What's next?

This page describes the generic way in which the **labs API** works.

In reality, most interesting objects are stored directly in their lab, so for most objects we only need two levels to reach them &ndash; the lab, then the object itself.

To work with actual objects, you'd have to learn each's type class' reference. At this moment Warehaus only supports three type classes:

* `builtin-lab` for lab types
* `builtin-server` for servers
* `builtin-cluster` for clusters of servers

*(coming soon)* We will cover each type class separately in the next pages.
