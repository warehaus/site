---
layout: docs-page
title: Servers
id: servers-guide
---

After creating a server type in a lab you'll have to install the Warehaus agent on the server.

# Agent download

When logged-in as an admin user, you're going to see a tab called **Adding Servers** on each type page. There you're going to find the URL to download the agent from.

For example, if your Warehaus instance is at `example.com`, your lab is named `lab` and your server type is named `server`, you'll download the agent from

    http://example.com/api/v1/labs/lab/~/server/agent.py

# Running the agent

After downloading the agent, make sure to run it as `root` as a daemon.

Immediately after starting the agent you should see your server in Warehaus.
