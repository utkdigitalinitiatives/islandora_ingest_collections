# Islandora Ingest Collection

A utility module for creating Islandora collection objects using this Delimiter Separated Value(DSV) files.


## Introduction

This module contains  a drush script for creating collections in an Islandora instance using 
data from a DSV file.

The DSV file must be contain the proper fields and the correct number of fields for the data to be ingested.

## Usage

### Step 1: Creating a DSV file

The DSV file must contain the following fields in the exact order shown below:

parentPID | objectNamespace | objectId | fedoraLabel | modsDescription | modsTypeOfResource | drupalContentType | thumbnailFilepath

The parentPID is the fedora parent pid of the new collection object to be created

The objectNamespace is namespace of the new collection object that will be concatentated with object_id to make a pid

The objectId is the object_id of the new collection object that will be concatentated with namespace to make a pid

The fedoraLabel is the fedora label of the object

The modsDescription is assigned to the MODS field description

The modsTypeOfResource is assigned to the MODS field type of resource

The drupalContentType is assigned to a Drupal Node's content type. This field may be left empty.

The thumbnailFilepath is the full path to the thumbnail used to represent the Collection. This field may be left empty

### Step 2: Importing collection objects into Islandora


Two examples of the drush command are:

```
drush --user=admin create-islandora-collections-from-dsv  --input=/tmp/collection_data.csv --create_node
```
or its short form:

```
drush --user=admin cicfd --input=/tmp/collection_data.csv --create_node
```

Options are:

 * `--input`: (Required) The absolute path to the delimiter separated file.
 * `--create_node`: (Optional) Create a node for each collection. Defaults to "page". The content type must exist and must be configured as described below.

The default delimiter for the delimiter separated file is ','.

The default enclosure for the delimiter separated file is '"'.

The default escape for the delimiter separated file is '"'.

Any of these settings may be modified by changing the global definitions in the islandora_ingest_collections.module file.

If there are no thumbnail images in the collection data directory, or if the drush script can't find an image identified in the delimiter separated file (due to a mismatching filename, for example), the newly created collection is assigned the default thumbnail image provided by the Islandora Collection Solution Pack.


## Creating Drupal nodes for collections

If you include the `--create_node` option, the drush script will create a Drupal node of the  content type specified in the DSV file. You must be certain that this content type exists or must create this content type *before* running the drush command. The content type must contain the following fields:

 * title
 * cdm_alias (field type = Text, widget = Text field)
 * description (field type = Long text, widget = Textarea (multiple rows); make the default input format "Plain Text")
 * thumbnail (field type = Image, widget = Image)
 * pid (field type = Text, widget = Text field)


The nodes will be published, not sticky, be owned by user ID 1, and use your site's default language. If you want to change these settings, you'll need to do so manually or using [Views Bulk Operations](https://www.drupal.org/project/views_bulk_operations). If the input data does not contain descriptions or thumbnails, values for these fields will not be added to the nodes.

## Requirements

This module requires the following modules/libraries:

* [Islandora](https://github.com/islandora/islandora)
* [Islandora Collection Solution Pack](https://github.com/Islandora/islandora_solution_pack_collection)

## Current maintainer

* [Robert Patrick Waltz](https://github.com/robert-patrick-waltz)

## License

[GPLv3](http://www.gnu.org/licenses/gpl-3.0.txt)
