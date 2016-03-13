---
layout: docs-page
title: Ownership API
id: ownership-api
---

This page covers the API for taking and releasing ownership from clusters.

*At this point only one owner is allowed per cluster.*

# Taking Ownership

Assuming our cluster is named `bench` in the `kim` lab, taking ownership for the `fred` user can be done by invoking a `POST` to the `owner` action of the cluster:

    curl -X POST -H "Content-Type: application/json" -H "Authentication-Token: ..." -d '{"username": "fred"}' http://example.com/api/v1/labs/kim/bench/owner

An example output:

    [{"obtained_at": "2016-02-15 05:54:26.067179+00:00", "owner_id": "eadd2a8c-fef1-48b1-99fa-4ea774b4677f"}]

Note that Warehaus returns the time when the ownership was taken and the `id` of the user who now owns the cluster. In this case it's `fred`. See the [Users API](users.md) for more information about user IDs.

Trying to take ownership again would result in a `409 CONFLICT` status, unless we try to take ownership for the currently owning user. In the latter case the same ownership information is returned with a `200 OK` status and no action is taken. Trying to assign to a nonexistent user results in `400 BAD REQUEST`.

# Releasing Ownership

Ownership can be released either by the owner or by an admin.

To release ownership, invoke a `DELETE` to the cluster's `owner` action:

    curl -X DELETE -H "Content-Type: application/json" -H "Authentication-Token: ..." http://192.168.99.100/api/v1/labs/kim/bench/owner

When successful a `204 NO CONTENT` with an empty response is returned. Trying to release a cluster with no ownership also results in `204 NO CONTENT`.
