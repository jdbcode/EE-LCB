---
title: Welcome
---

<img src='https://jdbcode.github.io/EE-LCB/assets/images/ee-lcb-logo.svg'>

## Earth Engine Landsat Collection Builder

A client library for the [Google Earth Engine](https://earthengine.google.com/) JavaScript API that standardizes 
Landsat collection building and pre-processing routines.

- Collection assembly
- Inter-sensing harmonization
- Cloud masking
- Transformations
- Quality assessment
- Mosaicking
- Visualization
- Sample datasets

Functions are designed to map over an image collection with a single task and be chained
together to complete a desired plan.

Here is an example plan that generates an annual cloud-free image time series
of mean summer NDVI 1984-2018:

```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

lcb.setProps({
  startYear: 1984,
  endYear: 2018,
  startDate: '07-01',
  endDate: '09-01',
  sensors: ['LT05', 'LE07', 'LC08'],
  cfmask: ['cloud', 'shadow'],
  harmonizeTo: 'LC08',
  aoi: ee.Geometry.Point([-110.438, 44.609])
});

var plan = function(year){
  var col = lcb.sr.gather(year)
    .map(lcb.sr.maskCFmask)
    .map(lcb.sr.harmonize)
    .map(lcb.sr.addBandNDVI)
    .select('NDVI');
  return lcb.sr.mosaicMean(col);
};

var years = ee.List.sequence(lcb.props.startYear, lcb.props.endYear);
var annualSummerMeanNDVI = ee.ImageCollection.fromImages(years.map(plan));
```

This examples completes the following steps:

1. Gathers Landsat surface reflectance images from TM, ETM+ and OLI for months July through August annually; 
2. Masks clouds and cloud shadows
3. Standardizes TM and ETM+ images to OLI;
4. Calculates NDVI
5. Makes a collection composed of annual mean NDVI composites

By way of example, EE-LCB wishes to promote development, documentation, and sharing of client libraries that
provide consistency and ease of accomplishment for processing steps of other datasets and applications.
There is a need to move past everyone writing essentially the same processing scripts one hundred different 
ways before getting to analyses. Let's get to the good stuff already! 

<img src='https://jdbcode.github.io/EE-LCB/assets/images/ee-lcb-logo.svg'>

![TC brightness change](https://jdbcode.github.io/EE-LCB/assets/images/brtnss_change_banner.jpg)


**This site is under construction.**

---

### License

Except as otherwise noted, the content in this repository is [licensed](https://jdbcode.github.io/EE-LCB/terms/ee-lcb-license.html) under the
[Creative Commons Attribution 4.0 International (CC BY 4.0) License](https://creativecommons.org/licenses/by/4.0/), and
code samples are licensed under the [Apache 2.0 License](http://www.apache.org/licenses/LICENSE-2.0).

The EE-LCB site renders content using [Just the Docs](https://github.com/pmarsceill/just-the-docs), 
a documentation theme for Jekyll, distributed under an [MIT License](https://jdbcode.github.io/EE-LCB/terms/ee-lcb-license.html). 






