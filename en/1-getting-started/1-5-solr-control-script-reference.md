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

|Parameter|description|Example|
|---|---|---|
|`-c <collection>`|Name of the collection to run a healthcheck against(required).|bin/solr healthcheck -c gettingstarted|
|`-z <zkhost>`|ZooKeeper connection string, defaults to localhost:9983. If you are running Solr on a port other than 8933, you will have to specify the ZooKeeper connection string. By default, this will be the Solr port + 1000.|bin/solr healthcheck -z localhost:2181|

Below is an example healthcheck request and response using a non-standard ZooKeeper connect string, with 2 nodes running:

    $ bin/solr healthcheck -c gettingstarted -z localhost:9865

    {
        "collection":"gettingstarted",
        "status":"healthy",
        "numDocs":0,
        "numShards":2,
        "shards":[
        {
            "shard":"shard1",
            "status":"healthy",
            "replicas":[
                {
                "name":"core_node1",
                "url":"http://10.0.1.10:8865/solr/gettingstarted_shard1_replica2/",
                "numDocs":0,
                "status":"active",
                "uptime":"2 days, 1 hours, 18 minutes, 48 seconds",
                "memory":"25.6 MB (%5.2) of 490.7 MB",
                "leader":true},
                {
                "name":"core_node4",
                "url":"http://10.0.1.10:7574/solr/gettingstarted_shard1_replica1/",
                "numDocs":0,
                "status":"active",
                "uptime":"2 days, 1 hours, 18 minutes, 42 seconds",
                "memory":"95.3 MB (%19.4) of 490.7 MB"
                }
            ]
        },
            {
            "shard":"shard2",
            "status":"healthy",
            "replicas":[
                {
                "name":"core_node2",
                "url":"http://10.0.1.10:8865/solr/gettingstarted_shard2_replica2/",
                "numDocs":0,
                "status":"active",
                "uptime":"2 days, 1 hours, 18 minutes, 48 seconds",
                "memory":"25.8 MB (%5.3) of 490.7 MB"},
                {
                "name":"core_node3",
                "url":"http://10.0.1.10:7574/solr/gettingstarted_shard2_replica1/",
                "numDocs":0,
                "status":"active",
                "uptime":"2 days, 1 hours, 18 minutes, 42 seconds",
                "memory":"95.4 MB (%19.4) of 490.7 MB",
                "leader":true}]}]}

------

## Collections and Cores

The `bin/solr` script can also help you create new collections (in SolrCloud mode) or cores (in standalone mode), or delete collections.

### Create

The `create` command detects the mode that Solr is running in (standalone or SolrCloud) and then creates a core or collection depending on the mode.

    bin/solr create [options]
    bin/solr create -help

### Available Parameters

|Parameter|Description|Example|
|---|---|---|
|`-c <name>`|Name of the core or collection to create (required).|bin/solr create -c mycollection|
|`-d <confdir>`|The configuration directory. This defaults to `data_driven_schema_configs`. See the section [Configuration Directories and SolrCloud]() below for more details about this option when running in SolrCloud mode.|bin/solr create -d basic_configs|
|`-n <configName>`|The cofniguration name. This defaults to the same name as the core or collection.|bin/solr create -n basic|
|`-p <port>`|Port of a local Solr instance ot send the create command to; by default the script tries to detect the port by looking for running Solr instances.<br/>This option is useful if you are running multiple standalone Solr instances on the same host, thus requiring you to be specific about which instance to create the core in.|bin/solr create -p 8983|
|`-s <shards>`<br/>`-shards`|Number of shards to split a collection into, default is 1; only applies when Solr is running SolrCloud mode.|bin/solr create -s 2|
|`-rf<replicas>`<br/>`-replicationFactor`|Number of copies of each document in the collection. The default is 1(no replication).|bin/solr create -rf 2|
|`-force`|If attempting to run create as "root" user, the script will exit with a warning that running Solr or actions against Solr as "root" can cause problems. It is possible to override this warning with the -force parameter.|bin/solr create -c foo -force|

## Configuration Directories and SolrCloud

Before creating a collection in SolrCloud, the configuration directory used by the collection must be uploaded to ZooKeeper. The create command supports serveral use cases for how collections and configuration directories work. The main decision you need to ake is whether a configuration directory in ZooKeeper should be shared across multiple collections.

Let's work through a few examples to illustrate hwo configuration directories work in SolrCloud.

First, if you don't provide the `-d` or `-n` options, then the defualt configuration ($SOLR\_HOME/server/solr/configsets/data\_driven\_schema\_configs/conf) is uploaded to ZooKeeper using the same name as the collection. For example, the following command will result in the data\_driven\_schema\_configs configuration being uploaded to `/config/contacts` in ZooKeeper: `bin/solr create -c contacts`. If you careate another collection, by doing `bin/solr create -c contacts2`, then another copy of the `data_driven_schema_configs` directory will be uploaded to ZooKeeper under `/configs/contacts2`. Any changes you make to the configuration for the contacts collection will not affect the contacts2 collection. Put simply, the default behavior creates a unique copy of the configuration directory for each collection you create.

You can override the name given to the configuration directory in ZooKeeper by using the `-n` option. For instance, the command `bin/solr create -c logs -d basic_configs -n basic` will upload the `server/solr/configsets/basic_configs/conf` directory to ZooKeeper as `/config/basic`.

Notice that we used the `-d` options to specify a different configuration than the default. Solr provides several built-in configurations under `server/solr/configsets`. However you can also provide the path to your own configuration deirctory using the `-d` option. For instance the command `bin/solr create -c mycoll -d /tmp/myconfigs`, will upload `/tmp/myconfigs` into ZooKeeper under `/config/mycoll`. To reiterate, the configuration directory is named after the collection unless you override it using the `-n` option.

Other collections can share the same configuration by specifying the name of the shared configuration using the `-n` option. For instance, the following command will create a new collection that shares the basic configuration created previously: `bin/solr create -c logs2 -n basic`.

## Data-driven Schema and Shared Configurations

The `data_driven_schema_configs` schema can mutate as data is indexed. Consequently, we recommend that you do not share data-driven configurations between collections unless you are certain that all collections should inherit the changes made when indexing data into one of the collections.

## Delete

The `delete` command detects the mode that Solr is running in (standalone or SolrCloud) and then deletes the specified core (standalone) or collection (SolrCloud) as apporpriate.

    bin/solr delete [options]
    bin/solr delete -help

If running in SolrCloud mode, the delete command checks if the configuration directory used by the collection you are deleting is being used by other collections. If not, then the configuration directory is also deleted from ZooKeeper. For Example, if you created a collection by doing `bin/solr create -c contacts`, then the delete command `bin/solr delete -c contacts` will check to see if the `/config/contacts` configuration directory is being used by any other collections. If not, then the `/configs/contacts` directory is removed from ZooKeeper.

### Available Parameters

|Parameter|Description|Example|
|---|---|---|
|`-c <name>`|Name of the core / collection to delete (required).|bin/solr detele -c mycoll|
|`-deleteConfig <true|false>`|Delete the configuration directory form ZooKeeper. The Default is true.<br/>If the configuration directory is being used by another collection, then it will not be deleted even if you pass `-deleteConfig` as true.|bin/solr delete -deleteConfig false|
|`-p <port>`|The port of a local Solr instance to send the delete command to. by default the script tries to detect the port by looking for running Solr instances.<br/>This option is useful if you are running multiple standalone Solr instances on the same host, thus requiring you to be specific about which instance to delete the core from.|bin/solr delete -p 8983|

## Authentication

The `bin/solr` script allows enabling or disabling Basic Authentication, allowing you to configure authentication from the command line.

Currently, this script only enables Basic Authentication, and is only available when using SolrCloud mode.

### Enabling Basic Authentication

The command `bin/solr auth enable` configures Solr to use Basic Authentication when accessing the User Interface, using `bin/solr` and any API requests.

> <i class="fa fa-lightbulb-o" style="color:#428b30;"></i> For more information about Solr's authentication plugins, see the section [Securing Solr](). For more information on Basic Authentication support specifically, see the section [Basic Authentication Plugin]().

The `bin/solr auth enable` command makes several changes to enable Basic Authentication:

* Creates a `security.json` file and uploads it to ZooKeeper. The `security.json` file will look similar to:

    {
        "authentication":{
        "blockUnknown": false,
        "class":"solr.BasicAuthPlugin",
        "credentials":{
            "user":"vgGVo69YJeUg/O6AcFiowWsdyOUdqfQvOLsrpIPMCzk= 7iTnaKOWe+Uj5ZfGoKKK2G6hrcF10h6xezMQK+LBvpI="}
        },
        "authorization":{
            "class":"solr.RuleBasedAuthorizationPlugin",
            "permissions":[
                {"name":"security-edit", "role":"admin"},
                {"name":"collection-admin-edit", "role":"admin"},
                {"name":"core-admin-edit", "role":"admin"}
            ],
            "user-role":{"user":"admin"}
        }
    }

* Adds two lines to `bin/solr.in.sh` or `bin/solr.in.cmd` to set the authentication type, and the path to `basicAuth.conf`:

    # The following lines added by ./solr for enabling BasicAuth
    SOLR_AUT_TYPE = "basic"
    SOLR_AUTHENTICATION_OPTS="_Dsolr.httpclient.config=/path/to/solr-6.6.0/server/solr/basicAuth.conf"

* Creates the file `server/solr/basicAuth.conf` to store the credential information that is used with `bin/solr` commands.

The command takes the following parameters:

`-credentials`

The username and password in the format of `username:password` of the initial user.

If you perfer not to pass the username and password as an argument to the scritp, you can choose the `-prompt` option. Either `-credentials` or -prompt` must be specified.

`-prompt`

If prompt is preferred, pass **true** as a parameter to reuest the script to prompt the user to enter a username and password.

Either `-credentials` or `-prompt` must be specified.

`-blockUnknown`

When true, blocks all unauthenticated users from accessing Solr. This defaults to false, which means unauthenticated users will still be able to access Solr.

`updateIncludeFileOnly`

When true, only the settings in `bin/solr.in.sh` or `bin/solr.in.cmd` will be updated, and `secturity.json` will not be created.

`-z`

Defines the ZooKeeper connect string. This is useful if you want to enable authentication before all your Solr nodes have come up.

`-d`

Defines the Solr server directory, by default `$SOLR_HOME/server`. It is not common to need to override the default, and is only needed if you have customized the $SOLR_HOME directory path.

`-s`

Defines the location of `solr.solr.name`, which by default is `server/solr`. If you have multiple instances of Solr on the same host, or if you have customized the `$SOLR_HOME` directory path, you likely need to define this.

### Disableing Basic Authentication

You can disable Basic Authentication with `bin/solr auth disable`.

If the `-updateIncludeFileOnly` option is set to true, then only the settings in `bin/solr.in.sh` or `bin/solr.in.cmd` will be updated, and `security.json` will not be removed.

If the `-updateIncludeFileOnly` option is set to false, then the settings in `bin/solr.in.sh` or `bin/solr.in.cmd` will be updated, and `security.json` will be removed. however, the `basicAuth.conf` file is not removed with either option.

-----------

## ZooKeeper Operations

The `bin/solr` script allows certain operations affecting ZooKeeper. These operations are for SolrCloud mode only.  The operations are available as sub-commands, which each have their own set of options.

    bin/solr zk [sub-command][options]

> <i class="fa fa-info-circle" style="color:#19407c;"></i> Solr should have heeen started at least once before issuing these commands to initialize ZooKeeper with the znodes solr expects. Once ZooKeeper is initialized, Solr doesn't need to be running on any node to use these commands.

### Upload a configuration Set

Use the `zk upconfig` command to upload one of the pre-configured configuration set or a customized configuration set to ZooKeeper.




