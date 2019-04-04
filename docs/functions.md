---
title: Functions
layout: default
has_children: false
nav_order: 3
---

# Functions

Most functions operate on a single ee.Image so that they can be easily mapped over an ee.ImageCollection.
They focus on a single task for maximum flexibility

They use a global dictionary with many properties, this allows for simply function mapping - all of the global properties
are exposed to a given function being mapped.

Function names generally follow a verb-noun pattern where camel case is used to concatenate words, 
where the first is always a verb is typically a noun or in some cases an adjective.
In some cases a third word is included.

Function are divided into several sub-modules based on Landsat data type. 

xx = applies to all Landsat data

sr = applies to Landsat surface reflectance

dn = applies to landsat digital number

toa = applies to landsat top of atmosphere

refl = applies to SR and TOA reflectance


## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


## Set properties

### setProps



## Collection building functions

### sr.gather()*

A quite wonderful function.


### sr.harmonize(img)*

### xx.exclude()

## Transformation functions

### ???

## Correction functions

### ??.correctTopoMinearat

### xx.correctSLCoff

###	xx.correct

## Masking functions

### sr.maskFmask(img)* ⇒ `ee.ImageCollection`

Applies CFmask cloud and shadow mask to Landsat surface reflectance image.

| Param  | Type                | Description  |
| ------ | ------------------- | ------------ |
| img  | `ee.Image`| An ee.Image object  |
| props.mask | `list` | A list of elements to be masked out. Options: 'cloud', 'shadow', 'water', 'snow' |

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

## Collection assessment functions

### sr.countValid

## Composite functions

### xx.mosaicPath*

### xx.mosaicMean

### xx.mosaicMedian

### xx.mosaicMedoid

### xx.mosaicTargetDOY

## Visualization

### refl.visualize543

### refl.visualize432

### refl.visualize321

### refl.visualizeTC


