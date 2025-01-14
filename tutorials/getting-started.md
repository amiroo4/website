---
layout: page
title: Getting started with CityJSON
parent: Tutorials
nav_order: 1
permalink: /tutorials/getting-started/
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Download a simple file with 2 buildings

Download [twobuildings.city.json](../files/twobuildings.city.json), a simple file with 2 buildings.

You can open that file in any text editor to see its structure, and notice that you can manually edit it to change values and/or add new buildings, new metadata, or delete some attributes.

![](../files/gs-structure.png)


## Visualise it

Go to the [CityJSON official online viewer](https://viewer.cityjson.org) (called 'ninja') and drop the file, and voilà:

![](../files/gs-viewer.png)

You can also use more powerful viewers (where the semantics and attributes of the buildings are shown for instance), we offer a [list of viewers]({{ '/software/#viewers' | prepend: site.baseurl }}) and of other software.
One of them is [azul](https://itunes.apple.com/nl/app/azul/id1173239678?mt=12), a macOS-only viewer:

![](../files/gs-azul.png)


## Manipulate and edit it with cjio

Manually editing a file is error-prone, so instead you can use [cjio](https://github.com/cityjson/cjio).
It can be used to manipulate, edit, and validate CityJSON files.

You must have Python (version >3.7) installed, and [pip](https://pypi.org/project/pip/).

cjio is a command-line interface program, which means that there is no graphical user interface and that you need to use the console (also called the "Command Prompt", or the terminal).

To install the latest release:
```
pip install cjio
```

After the installation, you have a small program called `cjio`, to see its possibilities type `cjio --help` and it should print all the options, as shown here:

![](../files/gs-cjiohelp.png)


To get some general information about the file you downloaded, navigate to the folder where it is located, and type:

```
cjio twobuildings.city.json info
```

and you should get this output:
```
Parsing twobuildings.city.json
{
  "cityjson_version": "1.1",
  "epsg": null,
  "bbox": [
    300578.235,
    5041258.061,
    13.688,
    300618.138,
    5041289.394,
    29.45
  ],
  "cityobjects_1stlevel": 2,
  "cityobjects_present": {
    "Building": 2
  },
  "materials": false,
  "textures": false
}
```
Observe that the `"epsg"` is null.
To assign an [EPSG](https://epsg.io/) to the file (26918 is the value, that dataset is part of the [Montréal open 3D dataset](http://donnees.ville.montreal.qc.ca/dataset/maquette-numerique-batiments-citygml-lod2-avec-textures)), you can type:
```
cjio twobuildings.city.json crs_assign 26918 save twobuildings_reprojected.city.json
```
Notice that 2 cjio operators are used in one command: `crs_assign` and `save`.
The first one assigns the CRS to the file, and the second allows us to save the file on disk (in the current folder).

If you then type `cjio twobuildings_reprojected.city.json info` you should see that now an EPSG is assigned to 3D city model.

The cjio operators can be linked in a pipeline, and the 3D city model opened is passed through all the operators, and it gets modified by some operators.
Operators like `info` and `validate` output information in the console and just pass the 3D city model to the next operator.

Some examples:
```
$ cjio example.city.json subset --id house12 info materials_remove info save out.city.json
$ cjio example.city.json attribute_remove roofType save new.city.json
$ cjio myfile.city.json merge '/home/elvis/temp/*.json' save all_merged.city.json
```


## What else?

One important point is ensuring that the files you produce and manipulate are valid, see our [validation tutorial]({{ '/tutorials/validation/' | prepend: site.baseurl }}) to learn how to do this (for the schema and the geometric primitives).

Several datasets can be downloaded from the [datasets page]({{ '/datasets/' | prepend: site.baseurl }}), but more importantly, it is possible to convert any CityGML file, see [our tutorial]({{ '/tutorials/conversion/' | prepend: site.baseurl }}).


## Questions and need help?

[CityJSON has its own forum](https://github.com/cityjson/specs/discussions), don't hesitate to ask if you're struggling with reading/manipulating/creating CityJSON datasets.

