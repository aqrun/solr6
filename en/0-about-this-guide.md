# About This Guide

This guide describes all of the important features and functions of Apache Solr. It is free to download from http://lucene.apache.org/solr/.

Designed to provide high-level documentation, this guide is intended to be more encyclopedic and less of a cookbook. It is structured to address a broad apectrum of needs, ranging from new developers gettin started to well-experienced developers extending their application or troubleshooting. It will be of use at any point in the application life cycle, for whenever you need authoritative information about Solr.

The material as presented assumes that you are familiar with some basic search concepts and that you can read XML. It does not assume that you are a Java programmer, although knowledge of Java is helpful when working directly with Lucene or when developing custom extensions to a Lucene/Solr installation.

## Special Inline Notes

Special notes are included throughout these pages. There are several types of notes:

* Information blocks

> <i class="fa fa-info-circle" style="color:#19407c;"></i> These provide additional information that's useful for you to know.

* Important

> <i class="fa fa-bolt" style="color:#eeea74;"></i> This provide intormation that is critical for you to know.

* Tip

> <i class="fa fa-lightbulb-o" style="color:#428b30;"></i> These provide helpful tips.

* Caution

> <i class="fa fa-fire" style="color:#bf6900;"></i> These provide details on scenarios or configurations you should be careful with.

* Warning

> <i class="fa fa-exclamation-triangle" style="color:#bf0000;"></i> These are meant to warn you from a possibly dangerous change or action.


## Hosts and Port Examples

The default port when running Solr is 8983. The samples, URLs and screenshots in this guide may show different ports, because the port number that Solr uses is configurable. If you have not customized your installation of Solr, please make sure that you use port 8983 when following the examples, or configure your own installation to use the port numbers shown in the examples. For information about configuring port numbers, see [Managing Solr]().

Similarly, URL examples use 'localhost' throughout; if you are accessing Solr from location remote to the server hosting Solr, replace 'localhost' with the proper domain or IP where Solr is running.

## Paths

Path information is given relative to **solr.home**, which is the location under the main Solr installation where Solr's collections and their **conf** and **data** directories are stored. When running the various examples metioned through out this tutorial (i.e., bin/solr -e techproducts) the **solr.home** with be a sub directory of **example/** created for you automatically.

