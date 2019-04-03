---
title: Quickstart
layout: default
has_children: false
nav_order: 2
---


## Create a Landsat surface reflectance collection

```js
// load the EE-LCB module
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

// define some desired collection properties
var userProps = {
	startYear:1984,
	endYear:2018,
	startDay:'07-01',
	endDay:'09-01'
	mask:['cloud', 'shadow']
}

// set module properties
lcb.setProps(userProps)

// gather desired images into a collection
var srCol = lcb.sr.gather()

print(srCol)
```



The most important thing about this module is the `props` dictionary.
The `props` dictionary is...
















