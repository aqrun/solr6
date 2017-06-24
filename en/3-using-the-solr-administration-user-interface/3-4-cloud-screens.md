# 3.4. Cloud Screens

When running in [SolrClound]() mode, a "Cloud" option will appear in the Admin UI between UI between Logging and Collections/Core Admin.

This screen provides status information about each collection & node in your cluster, as well as access to the low level data being stored in ZooKeeper.

> Only Visible When using SolrCloud <br/> The "Cloud" menu option is only available on Solr instances running in SolrCloud mode Single node or master/slave replication instances of Solr will not display this option.

Click on the Cloud option in the left-hand navigation, and a small sub-menu appears with options call "Tree", "Graph", 'Graph(Radial)" and 'Dump". The default view ("Graph") shows a graph of each collection, the shards that make up those collections, and the addresses of each replica for each shard.

This example shows the very simple two-node cluster created using the `bin/solr -e cloud -noprompt` example command. In addition to the 2 shard, 2 replica "gettingstarted" collection, there is an additional "films" collection consisting of a single shard/replica:

The "Graph(Radial)" option provides a different visual view of each node. Using the same example cluster the radial graph view looks like:

The Tree option shows a directory structure of the data in ZooKeeper, including cluster wide information regarding the `live_nodes` and overseer status, as well as collection specific information such as the `state.json`, current shard leaders. and configuration file in use. In this example, we see the `state.json` file definition for the films collection:

The final option is "Dump", Which returns a JSON document containing all nodes, their contents and their children(recursively). This can be used to export a snapshot of all the data that Solr has kept inside Zookeeper and can aid in debugging solrcloud problems.