# 3.5. Collections / Core Admin

The Collections screen provides some basic functionality for managing your Collections, powered by the Collections API.

> If you are running a single node Solr instance, you will not see a Collections option in the left nav menu of the Amin UI.<br/> You will instead see a "Core Admin" screen that supports some comparable Core level information & manipulation via the CoreAdmin API instead.

The main display of this page provides a list of collections that exist in your cluster Clicking on a collection name provides some basic metadata about how the collection is defined, and its current shard & replicas, with options for adding and deleting individual replicas.

The buttons at the top of the screen let you make various collection level changes to your cluster, from add new collections or aliases to reloading or deleting a single collection.

Replicas can be deleted by clicking the red "X" next to the replica name.

If the shard is inactive, for example after a [SPLITSHARD action](), an option to delete the shard will appear as a red "X" next to the shard name.