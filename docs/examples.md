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

MSS - all MSS sensor images
LT - all TM sensor images (LT04 and LT05)
LE07 - Landsat 7 ETM+
LC08 - Landsat 8 OLI
SR - surface reflectance
TOA - Top of atmosphere reflectance
DN - digital number 
COL - image collection (ee.ImageCollection)
PROPS - properties
AOI - area-of-interest



## Collection assembly

--------------------------------------------------------------------------------------------

### Make a Landsat 8 surface reflectance collection

```js
var colProps = {
	startYear: 1984,
	endYear: 2018,
	startDay: '07-01',
	endDay: '09-01'
	sensors: ['LC08'],
	aoi: ee.Geometry.Point([-110.438, 44.609])
}
lcb.setProps(colProps)
var col = lcb.sr.gather()
```

--------------------------------------------------------------------------------------------

### Make a L7 SR COL

```js
var colProps = {
	startYear: 1984,
	endYear: 2018,
	startDate: '07-01',
	endDate: '09-01'
	sensors: ['LE07'],
	aoi: ee.Geometry.Point([-110.438, 44.609])
}
lcb.setProps(colProps)
var col = lcb.sr.gather()
```

--------------------------------------------------------------------------------------------

### Make an L5 SR COL

```js
var colProps = {
	startYear: 1984,
	endYear: 2018,
	startDate: '07-01',
	endDate: '09-01'
	sensors: ['LT05'],
	aoi: ee.Geometry.Point([-110.438, 44.609])
}
lcb.setProps(colProps)
var col = lcb.sr.gather()
```

--------------------------------------------------------------------------------------------

### Make an L8 SR COL for summer 2016

```js
var colProps = {
	startYear: 2018,
	endYear: 2018,
	startDate: '07-01',
	endDate: '09-01'
	sensors: ['LC08'],
	aoi: ee.Geometry.Point([-110.438, 44.609])
}
lcb.setProps(colProps)
var col = lcb.sr.gather()
```

--------------------------------------------------------------------------------------------







