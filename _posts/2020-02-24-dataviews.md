---
layout: article
title: Data Views cheatsheet
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#4c4cc2'
  background_image:
    gradient: 'linear-gradient(313deg, rgba(2,0,36, .6) 0%, rgba(76,76,194, .6) 47%, rgba(0,212,255, .6) 100%)'
    src: https://github.com/alexma2344/sitio/blob/master/assets/images/rainbows.jpg?raw=true"
sharing: true
comment: true
articles:
  excerpt_type: html
tags: netbrain dataview
---

<!--more-->

---

# Data Views

*"A visual representation of data sets, created using CLI, SNMP and/or API"*

It replaces [static worksheets](http://networkdiagram101.com/?page_id=113) in Visio with Dynamic Views. (It saves manual labour).


<img src="https://github.com/alexma2344/sitio/blob/master/assets/images/dataviews-improvement.jpg?raw=true">


## Implementation

- Instant Qapp
	- a **local** data view is created using a parser.

- On-demand Qapp
	- a Qapp is run on a map

- Scheduled Qapp
	- Creates a **Global** data view

- Data View Template
	- on-demand view that will use the latest data in the DB (not live network)

### Global vs Local

The diference between the two, is where the data is gathered from.

#### Local
Only visible on the current map.
It has a set of static values, that don't change because they were generated based on a "snapshot" at a given time.

#### Global
Visible from any map (where data is available for the devices).
Gets its information from the current baseline.