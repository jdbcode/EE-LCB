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

1. TOC
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



