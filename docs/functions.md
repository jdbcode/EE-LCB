---
title: Functions
layout: default
has_children: false
nav_order: 3
---

# Functions
{:.no_toc}

Most functions operate on a single ee.Image so that they can be easily mapped over an ee.ImageCollection.
They focus on a single task for maximum flexibility

They use a global dictionary with many properties, this allows for simply function mapping - all of the global properties
are exposed to a given function being mapped.

Function names generally follow a verb-noun pattern where camel case is used to concatenate words, 
where the first is always a verb is typically a noun or in some cases an adjective.
In some cases a third word is included.

Function are divided into several sub-modules based on Landsat data type. 

| Sub-module  | Applies to |
| :- | :- |
| ls   | All Landsat data |
| sr   | Landsat surface reflectance data |
| toa  | Landsat top of atmosphere |
| refl | SR and TOA reflectance |
| dn   | Landsat digital number |


## Table of contents
{:.no_toc .text-delta}

* TOC
{:toc}


## Set properties

### setProps()

&#10551; `dictionary`

Updates the "props" global dictionary with desired collection properties. Will print the
resulting "props" dictionary to the console.

| Param  | Type | Description |
| :- | :- | :- |
| colProps | `dictionary` | A dictionary specifying desired collection properties |

```js
var colProps = {
  startYear: 2013,
  endYear: 2018,
  startDate: '07-01',
  endDate: '09-01',
  sensors: ['LC08'],
  aoi: ee.Geometry.Point([-110.438, 44.609])
}
lcb.setProps(colProps)
```

[Try Live](http://example.com/)

--------------------------------------------------------------------------------------------

## Collection building functions

### sr.gather() 

&#10551; `ee.ImageCollection`

Gathers Landsat images into a collection. In the case where *LC08* images are included with either *LT05* or *LE07* images,
all images will be standardized to include only reflectance and CFmask *pixel_qa* bands 
and band names will match the *LC08* convention. In all other cases the original band arrangement and names 
will remain.

| Param  | Type | Description |
| :- | :- | :- |
| props.startYear | `int` | Minimum year in the desired range of the collection |
| props.endYear | `int` | Maximum year in the desired range of the collection |
| props.startDate | `string` | Minimum calendar date of the collection as 'mm-dd' (inclusive) |
| props.endDate | `string` | Maximum calendar date of the collection as 'mm-dd' (exclusive) |
| props.aoi | `ee.Geometry` | Area of interest used to filter image collection by bounds |
| props.sensors: | `list` | A list of the sensors to include images from. Options: 'LT05, 'LE07', 'LC08' |

```js
var colProps = {
  startYear: 2013,
  endYear: 2018,
  startDate: '07-01',
  endDate: '09-01',
  aoi: ee.Geometry.Point([-110.438, 44.609])
  sensors: ['LC08'],
}
lcb.setProps(colProps)
var col = lcb.sr.gather()
```

[Try Live](http://example.com/)

--------------------------------------------------------------------------------------------

### sr.harmonize(img)

### xx.exclude()

## Transformation functions

### ???

## Correction functions

### ??.correctTopoMinearat

### xx.correctSLCoff

--------------------------------------------------------------------------------------------

## Masking functions

### sr.maskFmask(img) ⇒ `ee.ImageCollection`

Applies CFmask cloud and shadow mask to Landsat surface reflectance image.

| Param  | Type                | Description  |
| ------ | ------------------- | ------------ |
| img  | `ee.Image`| An ee.Image object  |
| props.mask | `list` | A list of elements to be masked out. Options:<br> 'cloud', 'shadow', 'water', 'snow' |

```js
var myProps = {
	mask: ['cloud', 'shadow', 'water', 'snow']
}
lcb.setProps(myProps)
var col = lcb.sr.col().map(lcb.sr.maskFmask)
```

[Try Live](http://example.com/){: .btn }


### sr.maskOutliers*

### sr.maskDiff

--------------------------------------------------------------------------------------------

## Collection assessment functions

### sr.countValid

## Composite functions

### xx.mosaicPath*

### xx.mosaicMean

### xx.mosaicMedian

### xx.mosaicMedoid

### xx.mosaicTargetDOY

## Visualization

### sr.visualize654

&#10551; `ee.Image`

Creates a 8-bit RGB visualization image from Landsat 8 equivlent bands 6, 5, and 4 mapped to 
red, green, and blue, respectively.

Example: apply to ee.Image
{: .lh-0 .fs-1 }
```js
// 
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var img = lcb.sr.getLC08img();
var imgRGB = lcb.sr.visualize654(img)
Map.addLayer(imgRGB);
```

```js
// Example: apply to ee.ImageCollection
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var col = lcb.sr.getLC08col()
                .map(lcb.sr.visualize654)
Map.addLayer(col.first());
```


### sr.visualize543

### sr.visualize432

### sr.visualizeTC

## Example datasets

### sr.getLT05img()

&#10551; `ee.Image`

Returns an example *LANDSAT/LT05/C01/T1_SR* image.

```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var img = lcb.sr.getLT05img()
print(img)
```

--------------------------------------------------------------------------------------------

### sr.getLE07img()

&#10551; `ee.Image`

Returns an example *LANDSAT/LE07/C01/T1_SR* image. Note that the images is SLC-off.

```js
lcb.sr.getLE07img()
```

--------------------------------------------------------------------------------------------

### sr.getLC08img()

&#10551; `ee.Image`

Returns an example *LANDSAT/LC08/C01/T1_SR* image.

```js
lcb.sr.getLC08img()
```

--------------------------------------------------------------------------------------------

### sr.getLT05col()

&#10551; `ee.ImageCollection`

Returns an example *LANDSAT/LT05/C01/T1_SR* image collection.

```js
lcb.sr.getLT05col()
```

--------------------------------------------------------------------------------------------

### sr.getLE07col()

&#10551; `ee.ImageCollection`

Returns an example *LANDSAT/LE07/C01/T1_SR* image collection. Note that the images are SLC-off.

```js
lcb.sr.getLE07col()
```

--------------------------------------------------------------------------------------------

### sr.getLC08col()

&#10551; `ee.ImageCollection`

Returns an example *LANDSAT/LC08/C01/T1_SR* image collection.

```js
lcb.sr.getLC08col()
```

--------------------------------------------------------------------------------------------

### sr.getL578col()

&#10551; `ee.ImageCollection`

Returns an example image collection with one entry from:
- *LANDSAT/LT05/C01/T1_SR*
- *LANDSAT/LE07/C01/T1_SR*
- *LANDSAT/LC08/C01/T1_SR*

```js
lcb.sr.getL578col()
```

--------------------------------------------------------------------------------------------
