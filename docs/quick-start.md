---
layout: docs-page
title: Quick Start
id: quick-start
---

# Installing Warehaus

Warehaus is distributed as a Docker container.

Before starting Warehaus, you'd first have to setup a RethinkDB instance. The fastest way to do that is to run it in a Docker container. You can then link that container to Warehaus:

    docker run --name rethinkdb rethinkdb

This command line creates a new RethinkDB container. Note that this is great for testing and small deployments -- see the full documentation for this RethinkDB image in [this Docker Hub page](https://hub.docker.com/_/rethinkdb/).

Once you have a RethinkDB container, you can run the Warehaus container:

    docker run -p 80:80 --link rethinkdb warehaus/warehaus
    
This runs Warehaus while linking it to the RethinkDB container and exposes port 80.

## Existing RethinkDB Database

If you have an existing RethinkDB database, you can link using the following environment variables when running the Warehaus container:

* `RETHINKDB_PORT_28015_TCP_ADDR` sets the RethinkDB hostname.
* `RETHINKDB_PORT_28015_TCP_PORT` sets the RethinkDB port.
* `RETHINKDB_AUTH` sets the authentication string. *default: empty*
* `RETHINKDB_DB` sets the database name. *default: `warehaus`*
