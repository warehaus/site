---
layout: docs-page
title: Labs
id: labs-guide
---

# What's a lab?

Any hardware in Warehaus has to belong to some lab.

Think of a lab as an abstraction of a data-center: In most cases you'd just create one lab for each data-center you have, but sometimes you might want to create multiple labs in the same data-center or one lab for all your data-centers. Normally you won't need more than one lab, but we'll cover more advanced use-cases later on.

# Creating the first lab

After your first login you'll see a message for creating your first lab:

![Create First Lab](/assets/img/labs-create-first.png)

Click on the **Create a New Lab** button to create a new lab. In this document we're going to create a lab called `Kim`:

![Create Lab](/assets/img/labs-create.png)

Once your lab is created, you'll see the lab selector on the top left:

![Lab Selector](/assets/img/labs-selector.png)

The lab selector allows you to switch labs and also create new labs if you're an admin.

## Slugs / API Names

As you can see in the example above, while creating the first lab there's an **API name** generated for the lab.

This name, also called **slug**, is used in API calls related to this lab. Slugs are generated automatically from the display name for labs and other similar objects we'll see later on.

___

# What's Next?

In the next page we'll learn about **hardware types** and see how to add hardware to labs.
