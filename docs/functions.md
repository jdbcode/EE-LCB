---
title: Functions
layout: default
has_children: false
nav_order: 3
---

# Functions
{:.no_toc}

## Table of contents
{:.no_toc .text-delta}

* TOC
{:toc}


## **Set properties**

### setProps(colProps)

&#10551; `dictionary`

Updates the "props" global dictionary with desired collection properties. Upon execution it 
will print the current state of the "props" dictionary to the console. The "props" dictionary is 
accessible as `lcb.props`.

| Param  | Type | Description |
| :- | :- | :- |
| colProps | `dictionary` | A dictionary specifying desired collection properties |

Example. [Try Live](https://code.earthengine.google.com/5ada6a4d9f37a0476c7e4c1ee1882d3b)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var colProps = {
  startYear: 2013,
  endYear: 2018,
  startDate: '07-01',
  endDate: '09-01',
  sensors: ['LC08'],
  aoi: ee.Geometry.Point([-110.438, 44.609])
};
lcb.setProps(colProps);
```

--------------------------------------------------------------------------------------------

## **Collection building**

--------------------------------------------------------------------------------------------

### sr.gather(*year*) 

&#10551; `ee.ImageCollection`

Gathers Landsat images into a collection. In the case where *LC08* images are included with either *LT05* or *LE07* images,
all images will be standardized to include only reflectance and CFmask *pixel_qa* bands 
and band names will match the *LC08* convention. In all other cases the original band arrangement and names 
will remain. If the optional *year* argument is provided and no images are found to match the collection properties,
a blank filler image matching the bands of an actual image will be included in the returned collection. This is done
to avoid errors when making mosaics from annual collections. These filler images and their derivitives can be removed
with the `sr.removeFiller` function. Adds a number of properties to each image - TODO: list them. 

| Param  | Type | Description |
| :- | :- | :- |
| year | `int` | A year to filter images for a collection. When this is provided, the props.startYear and props.endYear are ignored and *year* used in their place |
| props.startYear | `int` | Minimum year in the desired range of the collection |
| props.endYear | `int` | Maximum year in the desired range of the collection |
| props.startDate | `string` | Minimum calendar date of the collection as 'mm-dd' (inclusive) |
| props.endDate | `string` | Maximum calendar date of the collection as 'mm-dd' (exclusive) |
| props.aoi | `ee.Geometry` | Area of interest used to filter image collection by bounds |
| props.sensors: | `list` | A list of the sensors to include images from. Options: 'LT05, 'LE07', 'LC08' |

Example: optional "year" argument not provided. [Try Live](https://code.earthengine.google.com/fa2007d24c69dbf13799d56b271146c9)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var colProps = {
  startYear: 2013,
  endYear: 2018,
  startDate: '07-01',
  endDate: '09-01',
  sensors: ['LC08'],
  aoi: ee.Geometry.Point([-110.438, 44.609]),
};
lcb.setProps(colProps);
var col = lcb.sr.gather();
print(col);
```

Example: optional "year" argument provided. [Try Live](https://code.earthengine.google.com/0b7748bc76efa70478a93c55dd2a0138)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var colProps = {
  startDate: '07-01',
  endDate: '09-01',
  sensors: ['LC08'],
  aoi: ee.Geometry.Point([-110.438, 44.609]),
};
lcb.setProps(colProps);
var col = lcb.sr.gather(2013);
print(col);
```



--------------------------------------------------------------------------------------------

### sr.standardizeBands(img)

&#10551; `ee.Image`

Standardizes image to include only reflectance and CFmask *pixel_qa* bands with 
names following the *LC08* convention.

| Param  | Type | Description |
| :- | :- | :- |
| img | `ee.image` | A Landsat surface reflectance image |

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var img = lcb.sr.getLT05img();
var imgStandardized = lcb.sr.standardizeBands(img);
print(imgStandardized);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLT05col();
var colStandardized = col.map(lcb.sr.standardizeBands);                     
print(colStandardized);
```

--------------------------------------------------------------------------------------------

### sr.removeBandCFmask(img)

&#10551; `ee.Image`

Removes the CFmask *pixel_qa* bands from a Landsat surface reflectance image.

| Param  | Type | Description |
| :- | :- | :- |
| img | `ee.image` | A Landsat surface reflectance image |

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var img = lcb.sr.getLC08img();
var imgNoCFmask = lcb.sr.removeBandCFmask(img);
print(imgNoCFmask);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var colNoCFmask = col.map(lcb.sr.removeBandCFmask);                     
print(colNoCFmask);
```

--------------------------------------------------------------------------------------------

### sr.removeImageSLCoff(col)

&#10551; `ee.ImageCollection`

Removes LE07 scan line corrector (SLC)-off images from a Landsat surface reflectance image.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.image` | A Landsat surface reflectance collection |

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var colProps = {
  startYear: 1999,
  endYear: 2005,
  startDate: '07-01',
  endDate: '09-01',
  sensors: ['LE07'],
  aoi: ee.Geometry.Point([-110.438, 44.609])
};
lcb.setProps(colProps);
var col = lcb.sr.gather();
var colNoSLCoff = lcb.sr.removeSLCoff(col);
print(colNoSLCoff);
```

--------------------------------------------------------------------------------------------

### sr.removeImageList(col)

&#10551; `ee.ImageCollection`

Removes images included in 'exclude' property of the collection properties dictionary.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat collection |
| props.exclude | `list` | A list of Landsat `system:index` strings |

Example: apply to ee.ImageCollection. [Try Live](https://code.earthengine.google.com/0ee3aa54818f4e6e67721f514ed9aba2)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var colProps = {
  exclude: ['LT05_038029_20000707']
};
lcb.setProps(colProps);
var col = lcb.sr.getLT05col();
print(col);
var colExclude = lcb.sr.removeImageList(col);
print(colExclude);
```

--------------------------------------------------------------------------------------------

### sr.removeImageFiller(col)

&#10551; `ee.ImageCollection`

Removes images filled in by the `gather` function for years where no images existed between the 
dates provided in the collection properties dictionary. Performs a filter on the 'filler' property
added to images by the `gather` function.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat collection |

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 

lcb.setProps({
  startYear: 1984,
  endYear: 1985,
  startDate: '07-01',
  endDate: '09-01',
  sensors: ['LT05'],
  aoi: ee.Geometry.Point([-110.438, 44.609])
});

var col = lcb.sr.gather(1984).merge(lcb.sr.gather(1985));
print('Collection w/  filler size: '+col.size().getInfo());
var colNoFiller = lcb.sr.removeImageFiller(col);
print('Collection wo/ filler size: '+colNoFiller.size().getInfo());
```





--------------------------------------------------------------------------------------------

## **Corrections**

--------------------------------------------------------------------------------------------


### sr.resample(img)

&#10551; `ee.Image`

Resamples images by either bilinear or bicubic interpolation. It does not apply resampling to any
QA bands.

| Param  | Type | Description |
| :- | :- | :- |
| img | `ee.Image` | A Landsat surface reflectance image |
| props.resample | `String` | Interpolation mode. Options: 'bilinear', 'bicubic'

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var img = lcb.sr.getLC08img();
var imgResample = lcb.sr.resample(img);
print(imgResample);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var colResample = col.map(lcb.sr.resample);                     
print(colResample);
```


### sr.harmonize(img)

&#10551; `ee.Image`

Spectrally harmonize Landsat 5 and 7 to 8 or vise versa.

| Param  | Type | Description |
| :- | :- | :- |
| props.harmonizeTo | `string` | Options: 'LE07', 'LC08' |

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var img = lcb.sr.getLT05img();
var imgHarmonized = lcb.sr.harmonize(img);
print(img);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getL578col()
var colHarmonized = col.map(lcb.sr.harmonize);                     
print(colHarmonized);
```


### sr.topoCorrMinnaert(img)

&#10551; `ee.Image`

This is an implementation of the Minnaert topo correction that models k for every pixel as a function of slope, 
specific to each Landsat reflectance band. User can supply the DEM, by default the program will use "USGS/GMTED2010". 
Extremely low illumination regions can result is values being set to 0 for all bands. 
Note that sun zenith and azimuth are held constant for given a image. Function calls sr.standardizeBand, which 
standardizes image to include only reflectance and CFmask pixel_qa bands with names following the LC08 convention.
The resulting image will therefore will contain only these bands.
  
This is the paper from which the implementation was ported:
> Ge, H., Lu, D., He, S., Xu, A., Zhou, G., & Du, H. (2008). Pixel-based Minnaert correction method for reducing topographic effects on a Landsat 7 ETM+ image. 
> Photogrammetric Engineering & Remote Sensing, 74(11), 1343-1350. | 
> https://orst.library.ingentaconnect.com/content/asprs/pers/2008/00000074/00000011/art00003?crawler=true&mimetype=application/pdf

Changes image property: *topo_correction* from 'null' to 'minnaert'

| Param  | Type | Description |
| :- | :- | :- |
| props.demMinnaert | `string` | Path to DEM. Default: 'USGS/GMTED2010' |

Example: apply to ee.Image. [Try Live](https://code.earthengine.google.com/3ebbd7514e5ab367782826d9df9e63ae)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var img = lcb.sr.getLT05img();
var imgTopoCorr = lcb.sr.topoCorrMinnaert(img);
print(imgTopoCorr);
```

Example: apply to ee.ImageCollection. [Try Live](https://code.earthengine.google.com/1893aa3a278f419fe39318cd67d30675)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLT05col();
var colTopoCorr = col.map(lcb.sr.topoCorrMinnaert);                     
print(colTopoCorr);
```

--------------------------------------------------------------------------------------------

## **Masking**

--------------------------------------------------------------------------------------------

### sr.maskCFmask(img)

&#10551; `ee.Image`

Applies CFmask cloud and shadow mask to a Landsat surface reflectance image.

| Param  | Type                | Description  |
| ------ | ------------------- | ------------ |
| img  | `ee.Image`| An ee.Image object  |
| props.mask | `list` | A list of elements to be masked out. Options:<br> 'cloud', 'shadow', 'water', 'snow' |

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var colProps = {
	mask: ['cloud', 'shadow', 'water', 'snow']
};
lcb.setProps(colProps);
var img = lcb.sr.getLC08img();
var imgMasked = lcb.sr.maskCFmask(img);
print(imgMasked);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var colProps = {
	mask: ['cloud', 'shadow', 'water', 'snow']
};
lcb.setProps(colProps);
var col = lcb.sr.getLC08col();
var colMasked = col.map(lcb.sr.maskCFmask);
print(colMasked);
```

### sr.maskBuffer(img)

&#10551; `ee.Image`

Adds a circular buffer to the existing mask of a given image.

| Param  | Type                | Description  |
| ------ | ------------------- | ------------ |
| img  | `ee.Image`| An ee.Image object  |
| props.maskBuffer | `integer` | buffer width in meters |

Example: apply to ee.Image. [Try Live](https://code.earthengine.google.com/a418d7de29b7b9c2c49f8219243610cc)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var colProps = {
	maskBuffer: 150
};
lcb.setProps(colProps);
var img = lcb.sr.getLC08img();
var imgMasked = lcb.sr.maskCFmask(img);
var imgMaskedBuffer = lcb.sr.maskBuffer(imgMasked);

Map.centerObject(imgMasked, 11);
Map.addLayer(lcb.sr.visualize654(imgMaskedBuffer), {}, 'Buffered Mask');
Map.addLayer(lcb.sr.visualize654(imgMasked), {}, 'Mask');
```

Example: apply to ee.ImageCollection. [Try Live](https://code.earthengine.google.com/f52014be081882fa3d29131953eab4f3)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var colProps = {
  mask: ['cloud', 'shadow', 'water', 'snow'],
	maskBuffer: 150
};
lcb.setProps(colProps);
var col = lcb.sr.getLC08col();
var colMaskedBuffer = col.map(lcb.sr.maskCFmask)
                         .map(lcb.sr.maskBuffer);

Map.centerObject(colMaskedBuffer.first(), 11);
Map.addLayer(lcb.sr.visualize654(colMaskedBuffer.first()), {}, 'Buffered Mask');
```

--------------------------------------------------------------------------------------------

## **Transformations**

--------------------------------------------------------------------------------------------

### addBandTC(img)

&#10551; `ee.Image`

Applies the Tasseled Cap transformation to a Landsat surface reflectance image and adds bands:
'TCB', 'TCG', 'TCW', 'TCA'. Values for 'TCB', 'TCG', 'TCW' are a derivation of the input
image. If the image is not rescaled by the user, then the scale factor is 10,000. Values for
TCA are degress between TCB and TCG scaled by 100.


| Param  | Type                | Description  |
| ------ | ------------------- | ------------ |
| img  | `ee.Image`| An ee.Image object  |

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var img = lcb.sr.getLC08img();
var imgTrans = lcb.sr.addBandTC(img);
print(imgTrans);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var colMasked = col.map(lcb.sr.addBandTC);
print(colMasked);
```

--------------------------------------------------------------------------------------------

### addBandNBR(img)

&#10551; `ee.Image`

Applies the Normalized Burn Ratio transformation to a Landsat surface reflectance image and adds bands:
'NBR'.

| Param  | Type                | Description  |
| ------ | ------------------- | ------------ |
| img  | `ee.Image`| An ee.Image object  |

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var img = lcb.sr.getLC08img();
var imgTrans = lcb.sr.addBandNBR(img);
print(imgTrans);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var colMasked = col.map(lcb.sr.addBandNBR);
print(colMasked);
```

--------------------------------------------------------------------------------------------

### addBandNDVI(img)

&#10551; `ee.Image`

Applies the Normalized Difference Vegetation Index transformation to a Landsat surface reflectance image and adds bands:
'NDVI'.

| Param  | Type                | Description  |
| ------ | ------------------- | ------------ |
| img  | `ee.Image`| An ee.Image object  |

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var img = lcb.sr.getLC08img();
var imgTrans = lcb.sr.addBandNDVI(img);
print(imgTrans);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var colMasked = col.map(lcb.sr.addBandNDVI);
print(colMasked);
```

--------------------------------------------------------------------------------------------

### addBandNDSI(img)

&#10551; `ee.Image`

Applies the Normalized Difference Snow Index transformation to a Landsat surface reflectance image and adds bands:
'NDSI'.

| Param  | Type                | Description  |
| ------ | ------------------- | ------------ |
| img  | `ee.Image`| An ee.Image object  |

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var img = lcb.sr.getLC08img();
var imgTrans = lcb.sr.addBandNDSI(img);
print(imgTrans);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var colMasked = col.map(lcb.sr.addBandNDSI);
print(colMasked);
```

--------------------------------------------------------------------------------------------

### addBandNDMI(img)

&#10551; `ee.Image`

Applies the Normalized Difference Moisture Index transformation to a Landsat surface reflectance image and adds bands:
'NDMI'.

| Param  | Type                | Description  |
| ------ | ------------------- | ------------ |
| img  | `ee.Image`| An ee.Image object  |

Example: apply to ee.Image. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var img = lcb.sr.getLC08img();
var imgTrans = lcb.sr.addBandNDMI(img);
print(imgTrans);
```

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var colMasked = col.map(lcb.sr.addBandNDMI);
print(colMasked);
```

--------------------------------------------------------------------------------------------

## **Collection assessment**

--------------------------------------------------------------------------------------------

### sr.countValid(col)

&#10551; `ee.Image`

Reduce an image collection to the number of valid, non-masked pixels.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat image collection |

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var col = lcb.sr.getL578col().map(lcb.sr.maskCFmask);
var nValid = lcb.sr.countValid(col);
Map.addLayer(nValid, {min:1, max:3});
```

--------------------------------------------------------------------------------------------

## **Mosaicking**

--------------------------------------------------------------------------------------------

### sr.mosaicMean(col)

&#10551; `ee.ImageCollection`

Reduce an image collection to annual mosaics by mean.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat surface reflectance image collection |
| props.compositeDate | `string` | Date to assign to annual mosaic images. Format: 'mm-dd' |

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var mosCol = lcb.sr.mosaicMean(col);
print(mosCol);
```

--------------------------------------------------------------------------------------------

### sr.mosaicMedian(col)

&#10551; `ee.ImageCollection`

Reduce an image collection to annual mosaics by median.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat surface reflectance image collection |
| props.compositeDate | `string` | Date to assign to annual mosaic images. Format: 'mm-dd' |

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var mosCol = lcb.sr.mosaicMedian(col);
print(mosCol);
```

--------------------------------------------------------------------------------------------

### sr.mosaicMin(col)

&#10551; `ee.ImageCollection`

Reduce an image collection to annual mosaics by minimum.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat surface reflectance image collection |
| props.compositeDate | `string` | Date to assign to annual mosaic images. Format: 'mm-dd' |

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var mosCol = lcb.sr.mosaicMin(col);
print(mosCol);
```

--------------------------------------------------------------------------------------------

### sr.mosaicMax(col)

&#10551; `ee.ImageCollection`

Reduce an image collection to annual mosaics by minimum.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat surface reflectance image collection |
| props.compositeDate | `string` | Date to assign to annual mosaic images. Format: 'mm-dd' |

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var col = lcb.sr.getLC08col();
var mosCol = lcb.sr.mosaicMax(col);
print(mosCol);
```

--------------------------------------------------------------------------------------------

### ls.mosaicPath(col)

&#10551; `ee.ImageCollection`

Mosaics images from the same WRS-2 path and date.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat image collection |

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
var colProps = {
	startYear: 2018,
	endYear: 2018,
	startDate: '07-01',
	endDate: '09-01',
	sensors: ['LC08'],
	aoi: ee.Geometry.Polygon(
        [[[-115.73283108195818, 47.0864041328592],
          [-115.73283108195818, 40.76536820958065],
          [-106.37247951945818, 40.76536820958065],
          [-106.37247951945818, 47.0864041328592]]])
};
lcb.setProps(colProps);
var col = lcb.sr.gather();
var colMos = lcb.ls.mosaicPath(col);
print(colMos);
Map.addLayer(colMos.filter(ee.Filter.eq('distinct_path', '038_20180709')));
```

--------------------------------------------------------------------------------------------

### sr.mosaicMedoid(col)

&#10551; `ee.ImageCollection`

Reduce an image collection to annual mosaics by minimum.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat surface reflectance image collection |
| props.compositeDate | `string` | Date to assign to annual mosaic images. Format: 'mm-dd' |

Example: apply to ee.ImageCollection. [Try Live](http://example.com/)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
lcb.setProps({compositeDate: '07-15'});
var col = lcb.sr.getLC08col()
  .map(lcb.sr.standardizeBands)
  .map(lcb.sr.removeBandCFmask)
  .map(lcb.sr.addBandNDVI);
var colMos = lcb.sr.mosaicMedoid(col);
print(colMos);
```

--------------------------------------------------------------------------------------------

### sr.mosaicDOY(col)

&#10551; `ee.ImageCollection`

Reduce an image collection by nearness to selected target day-of-year. Selects the unmasked pixel 
having the least absolute difference between time of observation and user-defined target day-of-year. 
Adds a band named "DOY" to the resulting mosaic with values corresponding to the day-of-year for the 
selected pixels.

| Param  | Type | Description |
| :- | :- | :- |
| col | `ee.ImageCollection` | A Landsat surface reflectance image collection |
| props.doyTarget | `integer` | Target day-of-year |

Example: apply to ee.ImageCollection. [Try Live](https://code.earthengine.google.com/09c056bc6ae8a7f3a251b7ee0fc6a856)
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js');
lcb.setProps({doyTarget: 215});
var col = lcb.sr.getLC08col()
  .map(lcb.sr.standardizeBands);
var colMos = lcb.sr.mosaicDOY(col);
print(colMos);
```

--------------------------------------------------------------------------------------------

## **Visualization**

--------------------------------------------------------------------------------------------

### sr.visualize654(img)

&#10551; `ee.Image`

Creates a 8-bit RGB visualization image from Landsat 8-equivalent bands 6, 5, and 4 mapped to 
red, green, and blue, respectively.

Example: apply to ee.Image
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var rgbImg = lcb.sr.visualize654(lcb.sr.getLC08img());
Map.addLayer(rgbImg);
```

Example: apply to ee.ImageCollection
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var rgbCol = lcb.sr.getLC08col()
                   .map(lcb.sr.visualize654)
Map.addLayer(rgbCol.first());
```

--------------------------------------------------------------------------------------------

### sr.visualize543(img)

&#10551; `ee.Image`

Creates a 8-bit RGB visualization image from Landsat 8-equivalent bands 5, 4, and 3 mapped to 
red, green, and blue, respectively.

Example: apply to ee.Image
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var rgbImg = lcb.sr.visualize543(lcb.sr.getLC08img());
Map.addLayer(rgbImg);
```

Example: apply to ee.ImageCollection
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var rgbCol = lcb.sr.getLC08col()
                   .map(lcb.sr.visualize543)
Map.addLayer(rgbCol.first());
```

--------------------------------------------------------------------------------------------

### sr.visualize432(img)

&#10551; `ee.Image`

Creates a 8-bit RGB visualization image from Landsat 8-equivalent bands 4, 3, and 2 mapped to 
red, green, and blue, respectively.

Example: apply to ee.Image
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var rgbImg = lcb.sr.visualize432(lcb.sr.getLC08img());
Map.addLayer(rgbImg);
```

Example: apply to ee.ImageCollection
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var rgbCol = lcb.sr.getLC08col()
                   .map(lcb.sr.visualize432)
Map.addLayer(rgbCol.first());
```

--------------------------------------------------------------------------------------------

### sr.visualizeTC(img)

&#10551; `ee.Image`

Creates a 8-bit RGB visualization image from Landsat Tasseled Cap transformation brightness, 
greenness, wetness mapped to red, green, and blue, respectively.

Example: apply to ee.Image
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var rgbImg = lcb.sr.visualizeTC(lcb.sr.getLC08img());
Map.addLayer(rgbImg);
```

Example: apply to ee.ImageCollection
{: .lh-tight .fs-2 }
```js
var lcb = require('users/jstnbraaten/modules:ee-lcb.js'); 
var rgbCol = lcb.sr.getLC08col()
                   .map(lcb.sr.visualizeTC)
Map.addLayer(rgbCol.first());
```

--------------------------------------------------------------------------------------------

## **Example datasets**

--------------------------------------------------------------------------------------------

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
