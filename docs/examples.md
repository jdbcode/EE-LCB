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

<details><summary>Make a Landsat 8 surface reflectance collection</summary>
<a href="http://example.com">Link</a>

{% highlight js %}
var myProps = {
	startYear: 1984,
	endYear: 2018,
	startDay: '07-01',
	endDay: '09-01'
}
lcb.setProps(myProps)
var col = lcb.gather()
{% endhighlight %}

</details>


### Make a Landsat 5 surface reflectance collection

