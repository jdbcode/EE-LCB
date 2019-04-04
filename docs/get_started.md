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


## Philosophy


## Function arguments 

EE-LCB is a function mapping focused module. Each in the module is function is named and can only accept
a single collection object as an argument. This is very limiting if there are other parameters that a
given function requires. A single global dictionary is used to define the properties of the collection 
being built. 


## About Landsat


## Banding naming convention


## Create a Landsat surface reflectance collection


```js
var props = {
  startYear: 1986,
  endYear: 2018,
  startDate: '06-15',
  endDate: '09-15',
  aoi: ee.Geometry.Point([-110.438,44.609]),
  cfmask: ['cloud', 'shadow', 'snow', 'water'],
  sensors: ['LT05', 'LE07', 'LC08'],
  includeSLCoff: 'true',
  exclude: [],
  harmonizeTo: 'LE07', // 'LC08' 
  resample: 'nearest-neighbor',
  compositeDate: '08-01'
};
```

```js
// load the EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define some desired collection properties
var userProps = {
	startYear: 1984,
	endYear: 2018,
	startDate: '07-01',
	endDate: '09-01'
	cfmask: ['cloud', 'shadow']
}

// set module properties
lcb.setProps(userProps)

// gather desired images into a collection
var srCol = lcb.sr.gather()

print(srCol)
```



The most important thing about this module is the `props` dictionary.
The `props` dictionary is...
















