---
layout: docs-page
title: Users API
id: users-api
---

# Internal User ID

When a user successfully authenticates with Warehaus its details are copied into Warehaus' database.

Although users authenticate with a username, an internal ID is created for each user. This ID is used to actually identify the user in various API use-cases.

# User List

Getting the list of users can be done by

    curl -H "Content-Type: application/json" -H "Authentication-Token: ..." http://192.168.99.100/api/v1/auth/users

Example output:

    {
       "objects":[
          {
             "username":"fred",
             "first_name":"Fred",
             "roles":[
                "admin",
                "user"
             ],
             "id":"eadd2a8c-fef1-48b1-99fa-4ea774b4677f",
             "last_name":"Bloggs",
             "email":"fred@example.com"
          }
       ]
    }

# Current User

To get the information about the currently logged-in user:

    curl -H "Content-Type: application/json" -H "Authentication-Token: ..." http://192.168.99.100/api/v1/auth/self

Example output:

    {
       "username":"fred",
       "first_name":"Fred",
       "api_tokens":[
          "..."
       ],
       "roles":[
          "admin",
          "user"
       ],
       "id":"eadd2a8c-fef1-48b1-99fa-4ea774b4677f",
       "last_name":"Bloggs",
       "email":"fred@example.com"
    }
