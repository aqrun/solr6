# 3.8. Collection-Specific Tools

In the left-hand navigation bar, you will see a pull-down menu titled "Collection Selector" that can be used to access collection specific administration screens.

> **Only Visible When Using SolrCloud** The "Collection Selector" pull-down menu is only available on Solr instances running in [SolrCloud mode]()<br/> Single node or master/slave replication instances of Solr will no display this menu, instead the Collection specific UI pages described in this section will be available in the Core Selector pull-down menu.

Clicking on the Collection Selector pull-down menu will show a list of the collections in your Solr cluster, with a search box that can be used to find a specific collection by name. When you select a collection form the pull-down, the main display of page will display some basic metadata about the collection, and a secondary menu will appear in the left nav with links to additional collection specific administration screens.

The collection-specific UI screens are listed below, with a link to the sectionn of this guide to find out more:

* Analysis - lefts you analyze the adta found in specific fields.
* Dataimport - shows you information about the current status of the Data Import Handler.
* Documents - provides a simple form allowing you to execute various Solr indexing commands directly from the browser.
* Files - shows the current core configuration files such as `solrconfig.xml`
* Query - lets you submit a structured query about varius elements of a core.
* Stream - allows you to submit streaming expressions and see results and parsing explanations.
* Schema Browser - displays schema data in a browser window.
