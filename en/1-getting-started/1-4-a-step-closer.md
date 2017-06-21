# 1.4. A step Closer

You already have some idea of Solr's schema. This section describes Solr's home directory and other configuration options.

When Solr runs in an application server, it needs access to a home directory. The home directory contains important configuration information and is the place where Solr will store its index. The layout of the home directory will look a little different when you are running Solr in standalone mode vs when you are running in SolrCloud mode.

The crucial parts of the Solr home directory are shown in these examples:

* Standalone Mode

```
<solr-home-director>/
    solr.xml
    core_name1
        core.properties
        conf/
            solrconfig.xml
            managed-schema
        data/
    core_name2
        core.properties
        conf/
            solrconfig.xml
            managed-schema
        data/
```

* SolrCloud Mode

```
<solr-home-directory>/
    solr.xml
    core_name1/
        core.properties
        data/
    core_name2/
        core.properties
        data/
```

You may see other files, but the main ones you need to know are:

* **solr.xml** specifies configuration options for your Solr server instance. For more informatino on **solr.xml** see [Solr Cores and Solr.xml]()
* Per Solr Core:
    * **core.properties** defines spcific properties for each core such as its name, the collection the core belongs to, the location of the schema, and other parameters. For more deatils on **core.properties** see the section [Defining core.properties]()
    * **solrconfig.xml** controls high-level behavior. You can, for example, specify an alternate location for the data directory. For more information on **solrconfig.xml**, see [Configring solrconfig.xml]()
    * **managed-schema** (or **schema.xml** instead) describes the documents you will ask Solr to index. The Schema define a document as a collection of fields. You get to define both the field types and the fields themselves. Field type definitions are powerful and include information about how Solr processes incoming field values and query values. For more information on Solr Schemas, See [Documents, Fields, and Schema Design]() and the [Schema API]()
    * **data/** The directory containing the low level index files.

Note that the SolrCloud example does not include a **conf** directory for each Solr Core (so there is no **solrconfig.xml** or Schema file). This is because the configuration fles usually found in the **conf** directory are stored in ZooKeeper so they can be propagated across the cluster.

If you are using SolrCloud with the embedded ZooKeeper instance, you may also see **zoo.cfg** and **zoo.data** which are ZooKeeper cofniguration and data files. However, if you are running your own Zookeeper ensemble, you would supply your own ZooKeeper configuration file when you start it and the copies in Solr would be unused. For more information about ZooKeeper and SolrCloud, see the section [SolrCloud]().


