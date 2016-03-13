---
layout: docs-page
title: Hardware Types
id: hardware-types
---

Now that we have a lab, let's start adding hardware.

# Creating hardware types

Start by going into the lab configuration screen by clicking on the cog icon on the top-right. *Note that only admins can configure labs and see the cog icon.*

The first tab you'll see is the **Hardware Types** management tab:

![Hardware Types Tab](/assets/img/hardware-types-tab.png)

Then, select the type behavior, in our case it's a *Server*. You'll see the following screen:

![Create Hardware Type](/assets/img/hardware-types-create.png)

Here you can specify how the type should be called in singular and plural. Note that you can create multiple types of the same behavior as long as they have different API names. For example, you can create two types of servers: one for physical machines and one for virtual ones.

You can do the same for creating clusters as well.

After you create hardware types, each type appears as a tab in the lab:

![Type Tab](/assets/img/hardware-types-type-tab.png)

Creating multiple types would allow you to populate your hardware in multiple tabs by any category you see fit.

# What's next?

In the next section we'll see how to install the Warehaus agent on servers to have them monitored by Warehaus.
