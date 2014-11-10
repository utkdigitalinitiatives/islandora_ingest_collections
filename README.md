# Islandora CONTENTdm Collection Migrator

A utility module for exporting collection configuration data from a CONTENTdm instance and then creating Islandora collections using this data. This module will only be of use to sites that are migrating from CONTENTdm to Islandora.

## Introduction

This module has two main parts: 1) a command-line PHP script that harvests configuration data for collections on a CONTENTdm server and 2) a drush script for creating collections in an Islandora instance using that data. The PHP script outputs the collection data into a tab-separated file so it can be modified prior to being used by the drush script.

Detailed instructions for running the PHP script are provided within the script itself, but in a nutshell, oyou configure a few variables and then run the script. If you have access to your CONTENTdm server's shell and run the script there, the resulting data will contain the title and description for each CONTENTdm collection, plus the collection's thumbnail image. If you don't have access to your CONTENTdm server's shell (e.g., your CONTENTdm is hosted by OCLC), you can run the script from any computer that has PHP installed, but the output will only contain the collection titles. In both cases, you run the script by issuing the following command:

```
php get_collection_data.php
```

Running this script on the CONTENTdm server's command line creates a file containing one tab-delimited row per collection, for example (with tabs represented here by [\t]):

```
ubcCtn[\t]Chinatown News[\t]&lt;p&gt;The &lt;em&gt;Chinatown News&lt;/em&gt; was an English-language biweekly magazine.&lt;/p&gt;[\t]chn2.jpg
```

Thumbnail images identified in the last field are copied into the output directory within subdirectories named after the collection alias (the value in the first field). If run outside the CONTENTdm server, using the CONTENTdm Web API, the lines in the file only contain the first two fields, and the thumbnail images are not copied:
 
```
ubcCtn[\t]Chinatown News
```

Once you have copied the output from the script over to your Islandora server, you run the drush command on your Islandora server to create the collections identified in the output:

```
drush --user=admin create-islandora-collections-from-cdm --namespace=mynamespace --parent=mycollection:10  --input=/tmp/cdmcollectiondata/collection_data.tsv
```
or is short form:

```
drush --user=admin cicfc --namespace=mynamespace --parent=mycollection:10  --input=/tmp/cdmcollectiondata/collection_data.tsv
```

## Requirements

This module requires the following modules/libraries:

* [Islandora](https://github.com/islandora/islandora)

## Current maintainer

* [Mark Jordan](https://github.com/mjordan)

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
