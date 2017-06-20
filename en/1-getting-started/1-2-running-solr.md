# 1.2. Running Solr

This section describes how to run Solr with an example schema, how to add documents, and how to run queries. 

## start the Server

If you didn't start Solr after installing it , you can start it by running **bin/solr** from the Solr directory.

    $ bin/solr start

If youi are running Windows, you can start Solr by running **bin\solr.cmd** instead.

    bin\solr.cmd start
  
This will start Solr in the background, listening on port 8983.

When you start Solr in the background, the script will wait to make sure Solr starts correctly before returning to the command line prompt.

The **bin/solr** and **bin\solor.cmd** scripts allow you to customize how you start Solr. Let's work through a few examples of using the **bin/solr** script (if youi're running Solr on Windows, the **bin\solr.cmd** works the same as what is shown in the examples below):

### Solr Script Options

The **bin/solr** script has sveral options.

#### Script Help

To see how to use the **bin/solr** script, execute:

    $ bin/solr -help

For Specific usage instructions for the **start** command, do:

    $ bin/solr start -help

#### Start Solr in the Foreground

Since Solr is a server, it is more common to run it in the background, especially on Unix/Linux. However, to start Solr in the foreground, simply do:

    $ bin/solr start -f

If you are running Windows, you can run:

    bin\solr.com start -f

#### Start Solr with a Different Port

To change the prot Solr listens on, you can use the -p parameter when starting, such as:

    $ bin/solr start -p 8984

#### Stop Solr

When running Solr in the foreground (using -f), then you can stop if using Ctrl-c, however, when running in the background, you should use the **stop** command, such as:

    $ bin/solr stop -p 8983

The stop command requires you to spcify the port Solr is listening on or you use the -all parameter to stop all running Solr instances.

#### Start Solr with a Specific Bundled Example

Solr also provides anumber of useful examples to help you learn about key features. You can launch the examples using the -e flag. For instance, to launch the "techproducts" example, you would do:

    $ bin/solr -e techproducts

Currently, the available examples you can run are: techproducts, dih, shemaless, and cloud. See the sectiojn [Running with Example Configurations]() for details on each example.

<div style="background:rgba(255,255,0,0.2);border:1px solid rgba(255,255,0,0.5);padding:10px;">**Getting Started With SolrCloud<br/>
Running the *cloud* example starts Solr in SolrCloud mode. For more information on starting Solr in cloud mode, see the section [Getting Started with SolrCloud]()
</div>

#### Check if Solr is Running

if you're not sure if Solr is running locally, you can use the status command:

    $ bin/solr status

This will search for running Solr instances on your computer and then gather basic information about them, such as the version and memory usage.

That's it! Solr is runnig. If you need convincing, use a Web browser to see the Admin Console.

*http://localhost:8983/solr/*

![The Solr Admin interface]()

*The Solr Admin interface*

If Solr is not running, your browser will complain that it cannot connect to the server. Check your port number and try again.

## Create a Core

If you did not start Solr with an example configuration, you would need to create a core in order to be able to index and search. You can do so by running:

    $ bin/solr create -c <name>

This will create a core that uses a data-driven schema which tries to guess the correct field type when you add documents to the index.

To see all available options for createing a new core, execute:

    $ bin/solr create -help

## Add Cocuments

Solr is built to find documents that match queries. Solr's schema provides an idea of how content is structured (more on the schema later), but without documents there is nothing to find. Solr needs input before it can do much.

You may want to add a few sample documents before trying to index your own content. The Solr installation comes with different types of example documents located under the sub-directories of the **example/** directory of your installation.

In the **bin/** directory is the post scritp, a command line tool which can be used to index different types of documents. Do not worry too much about the details for now. The [Indexing and Basic Data Operations]() section has all the details on indexing.

To see som infomation about the usage of **bin/post**, use the **-help** option. Windows users, see the section for [Post Tool on Windows]().

**bind/post** can post varous types of content to Solr, including files in Solr's native XML and JSON formats, CSV files, a directory tree of rich documents, or even a simple short web crawl. See the example at the end of `bin/post -help` for varous commands to easily get started posting your content into Solr.

Go ahead and add the documents in some example XML files:

```
$ bin/post -c gettingstarted example/exampledocs/*.xml
SimplePostTool version 5.0.0
Posting files to [base] usr http://localhost:8983/solr/gettingstarted/update...
Entering auto mode. File endings considered are xml, json, csv, pdf, doc, docs, ppt, pptx, xls, xlsx, odt, odp, ods, ott, otp, ots, rtf, htm, html, txt, log
POSTing file gb18030-example.xml (application/xml) to [base]
POSTing file hd.xml (application/xml) to [base]
POSTing file ipod_video.xml (application/xml) to [base]
POSTing file manufacturers.xml (application/xml) to [base]
POSTing file mem.xml (application/xml) to [base]
POSTing file money.xml (application/xml) to [base]
POSTing file monitor.xml (application/xml) to [base]
POSTing file monitor2.xml (application/xml) to [base]
POSTing file mp500.xml (application/xml) to [base]
POSTing file sd500.xml (application/xml) to [base]
POSTing file solr.xml (application/xml) to [base]
POSTing file utf8-example.xml (application/xml) to [base]
POSTing file vidcard.xml (application/xml) to [base]
14 files indexed.
COMMITting Solr index changes to http://localhost:8983/solr/gettingstarted/update...
Time spent: 0:00:00.153
```

That's it! Solr has indexed the document contained in those files.

## Ask Questions

