Apache Solr Reference Guide
======

Covering Apache Solr 6.5

### Lincense

Lincensed to the Apache Software Foundation (ASF) under one or more contributor lincense agreements. See the NOTICE file distributed with this work for additional information regarding copyright owership. The ASF licenses this file to you under the Apache Lincense, Version 2.0 (the "License"); yoiu may not yuse this file except in compliance with the license. You may obtain a copy of the License at 

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing software distributed under the License is distributed on an "As IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

***Apache and the Apache feather logo are trademarks of The Apache Software Foundation. Apache Lucene, Apache Solr and their respective logos are trademarks of the Apache Software Foundation. Please see the Apache Trademark Policy for more information.***

*Fonts used in the Apache Solr Reference Guide include Raleway, licensed under the SIL Open Font License, 1.1.*

### Summary

This reference guide describes Apache Solr, the opence source solution for search. You can download Apache Solr from the Solr website at http://lucene.apache.org/solr/.

* **Getting Started**: This section guides you through the installation and setup of Solr.
* **Using the Solr Administration User Interface**: This section introduces the Solr Web-based user interface. From your browser you can view configuration files, submit queries, view logfile settings and Java environment settings, and monitor and control distributed configurations.
* **Documents, Fields, and Schema Design**: This section describes how Solr organizes its data for indexing. It explains how a Solr schema defines the fields and field types which Solr uses to orgtanize data within the document files it indexes.
* **Understanding Analyzers, Tokenizers, and Filters**: This section explains how Solr preepares text for indexing and searching. Analyzers parse text and produce a stream of tokens, lexical units used for indexing and searching. Tokenizers break field data down into tokens. Filters perform other transformational or selective work on token streams.
* **Indexing and Basic Data Operations**: This section describes the indexing process and basic index operations, such as commit, optimize, and rollback.
* **Searching**: This section presents an overview of the search process in Solr. It describes the main components used in searches, including request handlers, query parsers, and response writers. It lists the query parameters that can be passed to Solr, and it describes features such as boosting and faceting, which can be used to fine-tune search results.
* **The Well-Configured Solr Instance**: This section discusses performance tuning for Folr. It begins with an overview of the *solrconfig.xml* file, the tells you how to configure cores with *solr.xml*, how to configure the Lucene index writer, and more.
* **Managing Solr**: This section discusses important topics for running and monitoring Solr. Other topics include how to back up a Solr instance, and how to run Solr with Java Management Extensions(JMX).
* **SolrCloud**: This section describes the newest and most exciting of Solr's new features, SolrCloud, which provides comprehensive distributed capabilities.
* **Legacy Scaling and Distribution**: This section tells you hwo to grow a Solr distribution by dividing a large index into sections calld shards, which are the distributed across multiple servers, or by replicating a single index across multiple services.
* **Clent APIs**: This section tells you how to access Solr through various client APIs, includeing Javascript, JSON, and Ruby.

