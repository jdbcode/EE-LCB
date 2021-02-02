---
title: Get Started
layout: default
has_children: false
nav_order: 2
---

# Get Started
{:.no_toc}

## Table of contents
{:.no_toc .text-delta}

## EE-LCB<small>sr</small> refered to hereafter as LCB is a 


EE-LCB functions are designed to map over a collection of images with a single task and be
chained together to execute a processing plan. This architecture provides maximum flexiblity, simplicty and readibiltiy. The resulting collection can be used in time se

Load the module:

```
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');` 
```

1. Find the functions and put them together in a plan
2. Identify relevent collection properties for each of the functions
3. Set the properties
4. Gather images into a collection
5. Map functions over the collection

A typical workflow would include




EE-LCB functions are organized into groups that perform related tasks, they include

- Set properties
- Collection Building
- Corrections
- Masking
- Transformations
- Collection Assessment
- Mosaicking
- Visualization
- *There are also example datasets







## One variable to rule them all

Functions in this module take a single `ee.Image` or `ee.ImageCollection` as an argument. Other inputs that may be needed are provided by property values set in the `props` global dictionary, which is exposed to all functions. For instance, the cloud masking function `sr.maskCFmask()` takes a single ee.Image as an argument, however, the function also wants to know what image features (cloud, shadow, water, snow) you want to mask from the image. To specify these features, the `props.cfmask` property is given a feature `list` like this:

```
lcb.setProps({cfmask: ['cloud', 'shadow']})

```








## Band Naming Convention

The Landsat surface reflectance product is available for Landsat 5, 7, and 8 data. Landsat 5 and 7 have the same band configurations, but Landsat 8 is different (Fig 1). All images will be standardized to include only reflectance and CFmask *pixel_qa* bands and band names which match the *LC08* convention. In all other cases the original band arrangement and names will remain.   


| Band Name        | LT05 (TM)        | LE07 (ETM+)      | LC08 (OLI) |
| --- | --- | --- | --- |
| Ultra-blue band  | NA                | NA                | B1 (0.43-0.45 mm) |
| Blue band        | B1 (0.45-0.52 mm) | B1 (0.45-0.52 mm) | B2 (0.45-0.51) |
| Green band       | B2 (0.52-0.60)   | B2 (0.52-0.60)   | B3 (0.53-0.59) |
| Red band         | B3 (0.63-0.69)   | B3 (0.63-0.69)   | B4 (0.64-0.67) |
| NIR band         | B4 (0.76-0.90)   | B4 (0.77-0.90)   | B5 (0.85-0.88) |
| SWIR 1 band      | B5 (1.55-1.75)   | B5 (1.55-1.75)   | B6 (1.57-1.65) |
| SWIR 2 band      | B7 (2.08-2.35)   | B7 (2.09-2.35)   | B7 (2.11-2.29) |
| Cirrus band      | NA                | NA                | B8 (1.36-1.38) |
Figure 1. Landsat band properties


## Band Harmonization

Because Landsat 5 and 7 have slightly different widths than Landsat 8 for corresponding bands and because of other differences in sensor properties, there is a notable difference in measurement. We recommend harmonizing these data sets if they are to be mixed together in inter-sensor composites or time series analyses. We provide a standard linear transformation recommended by Roy et al 2016.

## Cloud masking

Landsat Surface reflectance products are delivered with masks from the CFmask algorithm that include features: cloud, cloud shadow, water, and snow. By using these masks to exclude cloud and cloud shadow images pixels, clear-view composites can be contructed by mosaicking intersecting images within a given time window (a couple of months during the summer season, for instance). EE-LCB provides a function to mask out these features from images and allows you to select which features to include in the mask. We recommend that this operation be performed before preforming spectral transformations or compositing.

## Transformations

Spectral transformations can compact variablity from two or more bands into a single band and also narrow variablity to an image feature, such as vegetation (NDVI) or brightness (Tasselled cap brightness).

## Images properties added

A number of image properties are added mostly for internal use by module functions, but you may find them of interest. Here are their definitions:

| Property | Type | Definition |
| --- | --- | --- |
| bands         | `string` | whether bands and their names are from the orginal data set ('original') or altered to match LC08 and include only reflectance and qa_pixel bands ('standardized') |
| band_names    | `string` | Whether band names are original ('!LC08') or LC08-equivalent ('LC08') |
| composite_year| `int`   | The year label given to a composite image |
| filler        | `string` | Is the image a blank representative for a composite year that had no images - 'yes' or 'no' |
| harmonized_to | `string` |  true, false,  'null' |
| sensing_year  | `int`   | The year of image aquisition, which can be different from the composite year, if the composite is allowed to cross the new year, for example in the case where images from December, Janruary, and February are allowed to be mosaicked as a southern hemisphere summer image composite. |

## Landsat

A survival guide to Landsat preprocessing
https://esajournals.onlinelibrary.wiley.com/doi/pdf/10.1002/ecy.1730

Current status of Landsat program, science, and applications
https://www.sciencedirect.com/science/article/pii/S0034425719300707
  












