# 3. Using the solr Administration User Interface

This section discusses the Solr Administration User Interface ("Admin UI").

The [Overview of the Solr Admin UI]() explains the basic features of the user interface, what's on the inital Admin UI page, and how to configure the interface. In addition, there are pages describing each screen of the Admin UI:

* Getting Assistance shows you how to get more information about the UI.
* [Logging]() > shows recent messages logged by this Solr node and provides a way to change logging levels for specific classes.
* [Cloud Screens]() display information about nodes when running in SolrCloud mode.
* [Collections /Solr Admin]() explains how to get management information about each core.
* [Java Properties]() shows the Java information about each core.
* [Thread Dump]() lets you see detailed information about each thread, along with state information. 
* [Collection-specific Tools]() is a section explaining additional screens available for each collection.
    * [Analysis]() - lets you analyze the data found in specific fields.
    * [Dataimport]() - shows you information about the current status of the Data Import Handler
    * [Documents]() - provides a simple form allowing you to execute various Solr indexing commands directly from the browser.
    * [Files]() - shows the current core configuration files such as `solrconfig.xml`.
    * [Query]() - lets you submit a structured query about various elements of a core.
    * [Stream]() - allows you to submit streaming expressions and see results and parsing explanations.
    * [Schema Browser]() - displays schema data in a browser window.
* [Core-specific Tools]() is a section exlaining additional screens available for each named core.
    * [Ping]() - lefts you ping a named core and datermine whether the core is active.
    * [Plugin/ stats]() - shows statistics for plugins and other installed components.
    * [Replication]() - shows you the current replication status for the core, and lets you enable/disable replication.
    * [Segments Info]() - Provides a visualization of the underlying Lucene index segments.
