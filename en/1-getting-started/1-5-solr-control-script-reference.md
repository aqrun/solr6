# 1.5. Solr Control Script Reference

You can start and stop Solr, create and delete collections or cores, perform operations on ZooKeeper and check the status of Solr and configured shards.

You can find the script in the **bin/** directory of your Solr installation. The **bin/solr** script makes Solr easier to work with by providing simple commands and options to quickly accomplish common goals.

More examples of **bin/solr** in use are available throughout the Solr Reference Guide, but particularly in the sections [Running Solr]() and [Getting Started with SlorCloud]().

## Starting and Stopping

### Start and Restart

The `start` command start Solr. The `restart` command allows you to restart Solr while it is already running or if it has been stopped already.

The `start` and `restart` commands have several options to allow you to run in SolrCloud mode, use an example configuration set, start with a hostname or port that is not the default and point to a local ZooKeeper ensemble.

    bin/solr start [options]
    bin/solr start -help
    bin/solr restart [options]
    bin/solr restart -help

When using the `restart` command, you must pass all of the parameters you initially passed when you started Solr. Behind the scenes, a stop request is initiated, so Solr will be stopped before being started again. If no nodes are already running, restart will skip the step to stop and process to starting Solr.

### Available Parameters

The `bin/solr` script provides many options to allow you to customize the server in common ways, such as changing the listening port. However, most of the defaults are adequate for most Solr installations, especially when just getting started.

| Parameter | Description | Example |
|-----------|-------------|---------|
|-a "\<string\>"| Start Solr with additional JVM parameters, such as those starting with -X. If you are passing JVM parameters that begin with "-D", you can omit the -a option.|bin/solr start -a "-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=1044"|
|-cloud| Start Solr in SolrCloud mode, which will also launch the embedded ZooKeeper instnce included with Solr. <br/> This option can be shortened to simply -c.<br/><br/>If you are already running a ZooKeeper ensemble that you want to use instead of the embedded (single-node) ZooKeeper, you should also pass the -z parameter. <br/> For more details, see the section [SolrCloud Mode]() below. | bin/solr start -c |
| -d \<dir\> | Define a server directory, defaults to `server` (as in, $SOLR_HOME/server). It is uncommon to override this option. When running multiple instances of Solr on the same host, it is more common to use the same server directory for each instance and use a unique Solr home directory useing the -s option. | bin/solr start -d newServerDir |
| -e \<name\> | Start Solr with an example are provided to help you get started faster with Solr generally, or just try a specific feature. 

The available options are:  

* cloud
* techproducts
* dih
* schemaless

See the section [Running with Example Configurations]() below for more details on the example configurations. | bin/solr start -e schemaless |
| -f | Start Solr in the foreground; you cannot use this option when running examples with the -e options. | bin/solr start -f |
| -h \<hostname\> | Start Solr with the defined hostname. If this is not specified, 'localhost' will be assumed. | bin/solr start -h search.mysolr.com |
| -m \<memory\> | Start Solr with the defined value as the min(-Xms) and max (-Xmx) heap size for the JVM. | bin/solr start -m 1g |
| -noprompt | Start Solr and suppress and prompts that may be seen with another option. This would have the side effect of acception all defaults implicitly.

For example, when using the "cloud" example, an interactive session guides you through several options for your SolrCloud cluster. If you want to accept all of the defaults, you can simply ad the -noprompt option to your request. | bin/solr start -e cloud -noprompt |
| -p \<port\> | Start Solr on the defined port. If this is not specified, '8983' will be used. | bin/solr start -p 8655 |
| -s <dir> | Sets the solr.solr.home system property; Solr will create core directories under this directory. This allows you to run multiple Solr instances on the same host while reusing the same server directory set using the -d parameter. If set, the specified directory should contain a solr.xml file, unless solr.xml exists in ZooKeeper. The default value is `server/solr`.

This parameter is ignored when running examples (-e), as the solr.solr.home depends on which example is run. | bin/solr start -s newHome|
| -v | Be more verbose. This changes the logging level of log4j from `INFO` to `DEBUG`, having the same effect as if you edited `log4j.properties` accordingly. | bin/solr start -f -v|
| -q | Be more quiet. This changes the logging level of log4j from `INFO` to `WARN`, having the same effect as if you edited `log4j.properties` accordingly. This can be useful in a production setting where you want to limit loggin to warnings and errors.| bin/solr start -f -q |
| -V | Start Solr with verbose messages from the start script | bin/solr start -V |
| -z \<zkHost\> | Start Solr with the defined ZooKeeper connection string. This options is only used with the -c option, to start Solr in SolrCloud||
||||
||||

