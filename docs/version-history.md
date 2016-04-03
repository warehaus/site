---
layout: docs-page
title: Version History
id: version-history
---

This page contains the release notes for the current, upcoming and past releases.

# Current and Upcoming

#### Version 0.1.1 *(current)*

* New 3-layer type system
* New RESTful API for types
* Cluster ownership
* New documentation

#### Version 0.1.2 *(upcoming)*

* Upgrading:
  * To upgrade from v0.1.1 please delete the `settings` table.
  * Objects and users will be kept but for new features you'd have to delete existing types and recreate them through the UI.
  * As LDAP is not supported in this release, you'd have to manually reset the password for existing users. If you have too many users, please wait for an LDAP-supporting release before upgrading.

* New Features:
  * Authentication:
    * New local user store
  * Agent improvements:
    * Send information about root filesystem, CPU and RAM
    * Server hardware is created with actual types and objects
  * Detailed server page showing all hardware
  * New editor for hardware types in lab management page
  * New user-attributes support
    * Define allowed attributes in lab management page for each hardware type, including sub-types such as PCI devices
    * Users can view and change attributes directly in object pages

* Changes:
  * Rebranding as Warehaus
  * New navigation design
  * Removed *first setup* procedure, a clean install comes with an `admin`:`admin` user
  * Authentication APIs are in their own process now

# Previous Releases

*none yet*
