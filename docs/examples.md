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

### Make a Landsat 7 surface reflectance collection

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

### Make a Landsat 5 surface reflectance collection

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

### Make Landsat 8 surface reflectance collection for a summer 2016

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







