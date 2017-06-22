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
|`-a "<string>"`| Start Solr with additional JVM parameters, such as those starting with -X. If you are passing JVM parameters that begin with "-D", you can omit the -a option.|bin/solr start -a "-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=1044"|
|-cloud| Start Solr in SolrCloud mode, which will also launch the embedded ZooKeeper instnce included with Solr. <br/> This option can be shortened to simply -c.<br/><br/>If you are already running a ZooKeeper ensemble that you want to use instead of the embedded (single-node) ZooKeeper, you should also pass the -z parameter. <br/> For more details, see the section [SolrCloud Mode]() below. | bin/solr start -c |
| `-d <dir>` | Define a server directory, defaults to `server` (as in, $SOLR_HOME/server). It is uncommon to override this option. When running multiple instances of Solr on the same host, it is more common to use the same server directory for each instance and use a unique Solr home directory useing the -s option. | bin/solr start -d newServerDir |
| `-e <name>` | Start Solr with an example are provided to help you get started faster with Solr generally, or just try a specific feature. <br/>The available options are: <br/>* cloud<br/>* techpraoducts<br/>* dih <br/>* schemaless <br/>See the section [Running with Example Configurations]() below for more details on the example configurations. | bin/solr start -e schemaless |
| -f | Start Solr in the foreground; you cannot use this option when running examples with the -e options. | bin/solr start -f |
| -h \<hostname\> | Start Solr with the defined hostname. If this is not specified, 'localhost' will be assumed. | bin/solr start -h search.mysolr.com |
| -m \<memory\> | Start Solr with the defined value as the min(-Xms) and max (-Xmx) heap size for the JVM. | bin/solr start -m 1g |
| -noprompt | Start Solr and suppress and prompts that may be seen with another option. This would have the side effect of acception all defaults implicitly.<br/>For example, when using the "cloud" example, an interactive session guides you through several options for your SolrCloud cluster. If you want to accept all of the defaults, you can simply ad the -noprompt option to your request. | bin/solr start -e cloud -noprompt |
| -p \<port\> | Start Solr on the defined port. If this is not specified, '8983' will be used. | bin/solr start -p 8655 |
| -s <dir> | Sets the solr.solr.home system property; Solr will create core directories under this directory. This allows you to run multiple Solr instances on the same host while reusing the same server directory set using the -d parameter. If set, the specified directory should contain a solr.xml file, unless solr.xml exists in ZooKeeper. The default value is `server/solr`.<br/>This parameter is ignored when running examples (-e), as the solr.solr.home depends on which example is run. | bin/solr start -s newHome|
| -v | Be more verbose. This changes the logging level of log4j from `INFO` to `DEBUG`, having the same effect as if you edited `log4j.properties` accordingly. | bin/solr start -f -v|
| -q | Be more quiet. This changes the logging level of log4j from `INFO` to `WARN`, having the same effect as if you edited `log4j.properties` accordingly. This can be useful in a production setting where you want to limit loggin to warnings and errors.| bin/solr start -f -q |
| -V | Start Solr with verbose messages from the start script | bin/solr start -V |
| `-z <zkHost>` | Start Solr with the defined ZooKeeper connection string. This options is only used with the -c option, to start Solr in SolrCloud mode. If this option is not provided, Solr will start the embedded ZooKeeper instance and use that instance for SolrCloud operations.| bin/solr start -c -z server1:2181, server2:2181 |
| -force | If Attempting to start Solr as the root user, the script will exit with a warning that running Solr as "root" can cause problems. It is possible to override this warning with the -force parameter. | sudo bin/solr start -force |

To emphasize how the default settings work take a moment to understand that the following commands are equivalent:

    bin/solr start
    bin/solr start -h localhost -p 8983 -d server -s solr -m 512m

### Setting Java System Properties

The `bin/solr` script will pass any additional parameters begin with `-D` to the JVM, which allows you to set arbitrary Java system properties.

For example, to set the auto soft-commit frequency to 3 seconds, you can do:

    bin/solr start -Dsolr.autoSoftCommit.maxTime=3000

### SolrCloud Mode

The `-c` and `-cloud` options are equivalent:

    bin/solr start -c
    bin/solr start -cloud

If you specify a ZooKeeper connection string, such as -z 192.168.1.4:2181, then Solr will connect to ZooKeeper and join the cluster.

If you do not specify the `-z` option when starting Solr in cloud mode, the Solr will launch an embedded Zookeeper server listening on the Solr port + 1000 i.e., if Solr is running on port 8983, then the embedded ZooKeeper will be listening on port 9983.

> <i class="fa fa-bolt" style="color:#eeea74;"></i> If you ZooKeeper connection string uses a chroot, such as `localhost:2181/solr`, then you need to create the /solr znode before launching SolrCloud using the `bin/solr` script. <br/> + To do this use the `mkroot` command outlined below, for example: `bin/solr zk mkroot/solr -z 192.168.1.4:2181`

When starting in SolrCloud mode, the interactive script session will prompt you to choose a configset to use. 

For more information about starting Solr in SolrCloud mode, see also the section [Getting Started with SolrCloud]()

### Running with example Configurations

    bin/solr start -e <name>

The example configurations allow you to get started quickly with a configuration that mirrors what you hope to accomplish with Solr.

Each example launches Solr with a managed schema, which allows use of the [Schema API]() to make schema edits, but does not allow manual editing of a Schema file If you would prefer to manually modify a `schema.xml` file directly, you can change this default as described in the section [Schema Factory Definition in SolrConfig]().

Unless otherwise noted in the descriptions below, the example do not enable [SolrCloud]() nor [schemaless mode]().

The following examples are provided:

* **cloud**: This example starts a 1-4 node SolrCloud cluster on a single machine. When chosen, an interactive session will start to guide you through options to select the initial configset to use, the number of nodes for your example cluster, the prots to use, and name of the collection to be created. When using this example, you can choose from any of the available configsets found in `$SOLR_HOME/server/solr/configsets`.

* **techproducts**: This example starts Solr in standalone mode with a schema designed for the sample documents included in the `$SOLR_HOME/example/exampledocs` directory. The configset used can be found in `$SOLR_HOME/server/solr/configsets/sample_techporducts_configs`.

* **dih**: This example starts Solr in standalone mode with the DataImportHandler(DIH) enabled and several example `dataconfig.xml` files pre-configured for different types of data supported with DIH (such as, database contents, email, RSS feeds, etc.). The configset used is customized for DIH, and is found in `$SOLR_HOME/example/example-DIH/solr/conf`. For more information about DIH, see the section [Uploading Structured Data Store Data with the Data Import Handler]().

* **schemaless**: This example starts Solr in standalon mode using a managed schema, as described in the section [Schema Factory Definition in SolrConfig](), and provides a very minimal pre-defined schema. Solr will run in [Schemaless Mode]() with this configuration, where Solr will create fields in the schema on the fly and will guess field types used in imcoming documents. The configset used can be found in `$SOLR_HOME/server/solr/configsets/data_driven_schema_configs`.

> <i class="fa fa-bolt" style="color:#eeea74;"></i> The run in-foreground option (`-f`) is not compatible with the `-e` option since the script needs to perform additional tasks after starting the Solr server.

### Stop

The `stop` command sends a STOP request to running Solr node, which allows it to shutdown gracefully. The command will wait up to 5 seconds for Solr to stop gracefully and then will forcefully kill the process (kill -9).

    bin/solr stop [options]
    bin/solr stop -help

### Available Parameters

|Parameter|Description|Example|
|---|---|---|
|`-p <port>`| Stop Solr running on the given port. If you are running more than one instance, or are running in SolrCloud mode, you either need to specify the ports in separate requests or use the -all option.|`bin/solr stop -p 8983`|
|`-all`|Stop all running Solr instances that have a valid PID.|bin/solr stop -all|
|`-k <key>`|Stop key used to protect from stopping Solr inadvertently; default is "solrrocks"|bin/solr stop -k solrrocks|

## System Information

### Version

The `version` command simply returns the version of Solr currently installed and immediatedly exists.

    $ bin/solr version
    X.Y.0

### Status

The `status` command displays basic JSON-formatted information for any Solr nodes found running on the local system.

The `status` command uses the `SOLR_PID_DIR` environment variable to locate Solr process ID files to find running Solr instances, which defaults to the `bin` directory.

    bin/solr status

The output will includes a status of each node of the cluster, as in this example:

    Found 2 Solr nodes:
    Solr process 39920 running on prot 7574
    {
        "solr_home":"/Applications/Solr/example/cloud/node2/solr/",
        "version":"X.Y.0",
        "startTime":"2015-02-10T17:19:54.739Z",
        "uptime":"1 days, 23 hours, 55 minutes, 48 seconds",
        "memory":"77.2 MB (%15.7) of 490.7 MB",
        "cloud":{
        "ZooKeeper":"localhost:9865",
        "liveNodes":"2",
        "collections":"2"
        }
    }

    Solr process 39827 running on port 8865
    {
        "solr_home":"/Applications/Solr/example/cloud/node1/solr/",
        "version":"X.Y.0",
        "startTime":"2015-02-10T17:19:49.057Z",
        "uptime":"1 days, 23 hours, 55 minutes, 54 seconds",
        "memory":"94.2 MB (%19.2) of 490.7 MB",
        "cloud":{
            "ZooKeeper":"localhost:9865",
            "liveNodes":"2",
            "collections":"2"
        }
    }

### Healthcheck

The `healthcheck` command generates a JSON-formatted health report for a collection when running on SolrCloud mode. The health report provides information about the state of every replica for all shards in a collection, including the number of committed documents and its current state.

    bin/solr helthcheck [options]
    bin/solr healthcheck -help

### Available Parameters




