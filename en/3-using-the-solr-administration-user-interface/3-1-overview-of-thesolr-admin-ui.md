# 3.1. Overview of the Solr Admin UI

Solr features a Web interface that makes it easy for Solr administrators and programmers to view [Solr Configuration]() details, run [queries and analyze]() document fields in order to fine-tune a Solr configuration and access [only documentation]() and other help.

**Figure 1. Solr Dashboard**

Accessing the URL `http://hostname:8983/solr/` will show the main dashboard, which is divided into towo parts.

A left-side of the screen is a menu under the Solr logo that provides the navigation through the screens of the UI. The first set of links are for system-level information and configuration and provide access to [Logging](), [Collection/Core Administration](), and [Java Properties](), among other things. At the end of this information is at least one pulldown listing Solr cores configured for this instance. On [SolrCloud]() nnodes, and additional pulldown list show all collections in this cluster. Clicking on a collection or core name shows secondary menus of information for the specified collection or core, such as a [Schema Browser](), [Config Files](), [Plugins & statistics](), and an ability to perform Queries on indexed data.

The center of the screen shows the detail of the option selected. This may include a sub-navigation for the option or text or graphical representation of the requested data. See the sections in this guide for each screen for more details.

Under the covers, the Solr Admin UI re-uses the same HTTP APIs available to all clients to access Solr-related data to drive an external interface.

> The path to the Solr Admin UI given above is `http://hostname:port/solr`, which redirects to `http://hostname:port/solr#/` in the current version. A convenence redirect is also supported, so simply accessing the Admin UI at `http://hostname:port/` will also redirect to `http://hostname:prot/solr/#/`

