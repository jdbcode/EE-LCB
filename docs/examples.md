---
title: Examples
layout: default
has_children: false
nav_order: 4
---

# Examples
{:.no_toc}

## Table of contents
{:.no_toc .text-delta}

* TOC
{:toc}



## Glossary

MSS - all MSS sensor images<br>
LT - all TM sensor images (LT04 and LT05)<br>
LE07 - Landsat 7 ETM+<br>
LC08 - Landsat 8 OLI<br>
SR - surface reflectance<br>
TOA - Top of atmosphere reflectance<br>
DN - digital number<br>
COL - image collection (ee.ImageCollection)<br>
PROPS - properties<br>
AOI - area-of-interest



## Collection assembly

--------------------------------------------------------------------------------------------

### Make a Landsat OLI SR collection Jul-Aug 2013-2018

Example [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define collection properties
var colProps = {
  startYear: 2013,
  endYear: 2018,
  startDate: '07-01',
  endDate: '09-01',
  sensors: ['LC08'],
  aoi: ee.Geometry.Point([-110.438, 44.609]
}

// set collection properties
lcb.setProps(colProps)

// gather images into an ee.ImageCollection
var col = lcb.sr.gather()

// print the collection
print(col)
```

--------------------------------------------------------------------------------------------

### Make a Landsat ETM+ SR collection July 2000

Example [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define collection properties
var colProps = {
  startYear: 2000,
  endYear: 2000,
  startDate: '07-01',
  endDate: '08-01'
  sensors: ['LE07'],
  aoi: ee.Geometry.Point([-110.438, 44.609])
}

// set collection properties
lcb.setProps(colProps)

// gather images into an ee.ImageCollection
var col = lcb.sr.gather()

// print the collection
print(col)
```

--------------------------------------------------------------------------------------------

### Make an Landsat TM collection Jul-Aug 1984-2012

Example [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define collection properties
var colProps = {
  startYear: 1984,
  endYear: 2012,
  startDate: '07-01',
  endDate: '09-01'
  sensors: ['LT05'],
  aoi: ee.Geometry.Point([-110.438, 44.609])
}

// set collection properties
lcb.setProps(colProps)

// gather images into an ee.ImageCollection
var col = lcb.sr.gather()

// print the collection
print(col)
```

--------------------------------------------------------------------------------------------

### Make a Landsat TM & ETM+ SR collection July 1999-2003

Example [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define collection properties
var colProps = {
  startYear: 1999,
  endYear: 2003,
  startDate: '07-01',
  endDate: '08-01'
  sensors: ['LT05', 'LE07'],
  aoi: ee.Geometry.Point([-110.438, 44.609])
}

// set collection properties
lcb.setProps(colProps)

// gather images into an ee.ImageCollection
var col = lcb.sr.gather()

// print the collection
print(col)
```

--------------------------------------------------------------------------------------------

### Make a Landsat TM, ETM+, and OLI SR collection July 1984-2018

Example [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define collection properties
var colProps = {
  startYear: 1984,
  endYear: 2018,
  startDate: '07-01',
  endDate: '08-01'
  sensors: ['LT05', 'LE07', 'LC08'],
  aoi: ee.Geometry.Point([-110.438, 44.609])
}

// set collection properties
lcb.setProps(colProps)

// gather images into an ee.ImageCollection
var col = lcb.sr.gather()

// print the collection
print(col)
```

--------------------------------------------------------------------------------------------

### Make a Landsat TM & ETM+ SR collection July 1984-2018 and mask out clouds & shadow

Example [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define collection properties
var colProps = {
  startYear: 1984,
  endYear: 2018,
  startDate: '07-01',
  endDate: '08-01'
  sensors: ['LT05', 'LE07'],
  mask: ['cloud', 'shadow'],
  aoi: ee.Geometry.Point([-110.438, 44.609])
}

// set collection properties
lcb.setProps(colProps)

// gather images into an ee.ImageCollection
var col = lcb.sr.gather().map(lcb.sr.maskCFmask)

// print the collection
print(col)
```

--------------------------------------------------------------------------------------------

### Make a Landsat TM, ETM+ and OLI SR collection July 1984-2018, mask out clouds & shadow & harmonize to OLI

Example [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define collection properties
var colProps = {
  startYear: 1984,
  endYear: 2018,
  startDate: '07-01',
  endDate: '08-01'
  sensors: ['LT05', 'LE07', 'LC08'],
  mask: ['cloud', 'shadow'],
  harmonizeTo: 'LC08',
  aoi: ee.Geometry.Point([-110.438, 44.609])
}

// set collection properties
lcb.setProps(colProps)

// gather images into an ee.ImageCollection
var col = lcb.sr.gather().map(lcb.sr.maskCFmask).map(lcb.sr.harmonize)

// print the collection
print(col)
```

--------------------------------------------------------------------------------------------

### Make an annual summer NDVI mean time series collection from Landsat TM, ETM+ and OLI SR 1984-2018

SOMETHING IS WRONG IF THERE IS A MISSING YEAR IN THE COLLECTION - TRY POINT: aoi: ee.Geometry.Point([-110.438, 44.609])

Example [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
// define collection properties
var colProps = {
  startYear: 1984,
  endYear: 2018,
  startDate: '07-01',
  endDate: '09-01',
  sensors: ['LT05', 'LE07', 'LC08'],
  cfmask: ['cloud', 'shadow'],
  harmonizeTo: 'LC08',
  aoi: ee.Geometry.Point([-122.8848, 43.7929])
};

// set collection properties
lcb.setProps(colProps);

// make an ee.List sequence from startYear to endYear
var years = ee.List.sequence(lcb.props.startYear, lcb.props.endYear);

// map over the list of years - return a list of annual mean NDVI composite images
var imgList = years.map(function(year){
  var col = ee.ImageCollection(lcb.sr.gather(year)
    .map(lcb.sr.maskCFmask)
    .map(lcb.sr.harmonize)
    .map(lcb.sr.addBandNDVI)
    .select('NDVI'));
  return lcb.sr.mosaicMean(col);
});

// convert the ee.List of annual mean NDVI composite images to an ee.ImageCollection
var meanNDVIcol = ee.ImageCollection.fromImages(imgList);

// print and map the resulting collection
print(meanNDVIcol);
Map.addLayer(meanNDVIcol);
```



--------------------------------------------------------------------------------------------

### Make an annual cloud-masked collection and count the number of clear pixels per year per pixel without counting row overlap

Example [Try Live](https://code.earthengine.google.com/752e31cc1f81eb4f4b3db10b367505c5)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define an AOI
var geometry = ee.Geometry.Polygon(
  [[[-112.71220703124999, 45.2238642695279],
    [-112.71220703124999, 44.2405035184126],
    [-108.64726562499999, 44.2405035184126],
    [-108.64726562499999, 45.2238642695279]]], 
    null, false
);

// define collection properties 
var colProps = {
  startYear: 2012,
  endYear: 2018,
  startDate: '06-20',
  endDate: '09-20',
  sensors: ['LE07', 'LC08'],
  cfmask: ['cloud', 'shadow', 'snow'],
  harmonizeTo: 'LC08',
  aoi: geometry,
};

// set collection properties
lcb.setProps(colProps);

// make a processing plan
var plan = function(year){
  var col = lcb.sr.gather(year)
    .map(lcb.sr.maskCFmask);
  return lcb.sr.countValid(lcb.ls.mosaicPath(col)).set('year', year);
};

// apply the processing plan to the range of years
var years = ee.List.sequence(lcb.props.startYear, lcb.props.endYear);
var nValidCol = ee.ImageCollection.fromImages(years.map(plan));

// subset one year to display
var nValid = nValidCol.filter(ee.Filter.eq('year', 2013));

// show on map
Map.centerObject(lcb.props.aoi, 7);
Map.addLayer(nValid, {
  palette:['#2C105C', '#711F81', '#B63679', '#EE605E', '#FDAE78'], 
  min:1, 
  max:25},
  'nValid'
);
```



### Get mean count of number of clear pixels per year per pixel without counting row overlap within a collection

Example [Try Live](https://code.earthengine.google.com/a21e3a8b52216da3011bd879d899894c)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define an AOI
var geometry = ee.Geometry.Polygon(
  [[[-112.71220703124999, 45.2238642695279],
    [-112.71220703124999, 44.2405035184126],
    [-108.64726562499999, 44.2405035184126],
    [-108.64726562499999, 45.2238642695279]]], 
    null, false
);

// define collection properties 
var colProps = {
  startYear: 2012,
  endYear: 2018,
  startDate: '06-20',
  endDate: '09-20',
  sensors: ['LE07', 'LC08'],
  cfmask: ['cloud', 'shadow', 'snow'],
  harmonizeTo: 'LC08',
  aoi: geometry,
};

// set collection properties
lcb.setProps(colProps);

// make a processing plan
var plan = function(year){
  var col = lcb.sr.gather(year)
    .map(lcb.sr.maskCFmask);
  return lcb.sr.countValid(lcb.ls.mosaicPath(col)).set('year', year);
};

// apply the processing plan to the range of years
var years = ee.List.sequence(lcb.props.startYear, lcb.props.endYear);
var nValidCol = ee.ImageCollection.fromImages(years.map(plan));

// reduce collection of nValid pixels to mean
var nValid = nValidCol.reduce(ee.Reducer.mean());

// show on map
Map.centerObject(lcb.props.aoi, 7);
Map.addLayer(nValid, {
  palette:['#2C105C', '#711F81', '#B63679', '#EE605E', '#FDAE78'], 
  min:1, 
  max:25},
  'nValid'
);
```


### Get an annual time series chart of N clear pixels for region summarized by percentiles

Example [Try Live](https://code.earthengine.google.com/9f1974309259da29ffe35a2cc42a05c7)
{: .lh-tight .fs-2 }
```js
// load EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define an AOI
var geometry = ee.Geometry.Polygon(
  [[[-112.71220703124999, 46.2238642695279],
    [-112.71220703124999, 44.2405035184126],
    [-108.64726562499999, 44.2405035184126],
    [-108.64726562499999, 46.2238642695279]]], 
    null, false
);

// define collection properties/ set 
var colProps = {
  startYear: 1984,
  endYear: 2018,
  startDate: '06-20',
  endDate: '09-20',
  sensors: ['LT05', 'LE07', 'LC08'],
  cfmask: ['cloud', 'shadow', 'snow'],
  harmonizeTo: 'LC08',
  aoi: geometry,
};

// set collection properties
lcb.setProps(colProps);

// make a processing plan
var plan = function(year){
  var col = lcb.sr.gather(year)
    .map(lcb.sr.maskCFmask);
  var nValid = lcb.sr.countValid(lcb.ls.mosaicPath(col)).set('year', year);
  var nValidSummary = nValid.rename(['nValid'])
    .reduceRegion({
      reducer:ee.Reducer.percentile([25,50,75]),
      geometry:lcb.props.aoi,
      scale:300,
      crs:'EPSG:5070',
      bestEffort:true,
      maxPixels:1e13,
      tileScale:3}
    );
  
  return ee.Feature(null, nValidSummary)
    .set('Year', ee.String(ee.Number(year).toShort()));
};

// apply the processing plan to the range of years
var years = ee.List.sequence(lcb.props.startYear, lcb.props.endYear);
var nValidSummary = ee.FeatureCollection(years.map(plan));

// show results
print(nValidSummary);
print(ui.Chart.feature.byFeature(nValidSummary, 'Year', ['nValid_p25', 'nValid_p50', 'nValid_p75']));
```

