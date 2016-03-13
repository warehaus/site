---
layout: docs-page
id: api-auth
title: Authentication API
---

API authentication can be done in two ways: JWT or using an authentication token. JWTs are not fully implemented at the moment and are used by the UI only, therefore we'll not cover them in this page.

Authentication tokens can be passed either through a query parameter or a header.

# Obtain an authentication token

First you'll need to create a token for yourself in the **Account** tab on the left. Once there, click on **API Tokens**:

![Create Token](/assets/img/auth-create-token.png)

You can create as many tokens as you need and use them as described below.

*Note: Admins can create tokens for other users through the **Admin** tab. This is useful for creating tokens for bot users.*

# Header token authentication *(recommended)*

To authenticate using a header, simply add the `Authentication-Token` header and put the authentication token as its value. For example:

    $ curl -H "Content-Type: application/json" -H "Authentication-Token: 64d54f54d4ab7bb9dce77f0b6187292eea25a3b6" http://192.168.99.100/api/v1/labs/kim/
    {"id": "d3a03a2d-8c2c-47b2-922a-8f1c037f9aed", "parent_id": null, "display_name": "Kim", "slug": "kim", "type_id": "8ffc25b1-7c7b-43ed-b92d-f388f470b7e6"}

*Don't forget to pass the `Content-Type` header as well*

# Query parameter token authentication *(not recommended)*

You can also pass an authentication token through the `token` query parameter. This is useful for testing things with your browser, however **this is an unsecure method as your token can be logged by a web server or a proxy**. Therefore you should prefer using the header method as shown above.

Example:

    curl -H "Content-Type: application/json" http://192.168.99.100/api/v1/labs/kim/?token=64d54f54d4ab7bb9dce77f0b6187292eea25a3b6
    {"id": "d3a03a2d-8c2c-47b2-922a-8f1c037f9aed", "parent_id": null, "display_name": "Kim", "slug": "kim", "type_id": "8ffc25b1-7c7b-43ed-b92d-f388f470b7e6"}

# Priority

Since multiple tokens can be specified in one API call, Warehaus checks for tokens in the following order:

1. Query parameter authentication token
1. Header authentication token
1. JWT token

If a valid token is found the user is logged-in and the rest of the tokens are ignored. If no token is found or an invalid token is passed, a `401 UNAUTHORIZED` error is returned.

*Note: the priority behavior may change in the future.*
