---
layout: article
title: NetBrain Essentials
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(313deg, rgba(2,0,36,1) 0%, rgba(76,76,194,1) 47%, rgba(0,212,255,1) 100%)'
    src: https://github.com/alexma2344/peperina/blob/master/assets/images/radiohead.jpg?raw=true"
sharing: true
comment: true
articles:
  excerpt_type: html
tags: computer-networks netbrain
---

<!--more-->

---

# NetBrain Essentials

Netbrain is an adaptative network automation [platform](https://en.wikipedia.org/wiki/Computing_platform). That brings dynamic maps and runbooks to our day to day IT operations.

With this platform you can reduce metrics such as:
- Mean time to repair (MTTR)
- Mean time between failures (MTBF)
- Mean time to discover a security incident
- Mean time to contain or mitigate a security incident
- Automating the provisioning of IT resources

## Dynamic Mapping

A dynamic [map](https://en.wikipedia.org/wiki/Map) is an interactive depiction, that emphasizes the relationship between **elements** on a **space**. 

In our case:
- **Elements**: Network nodes
- **Space**: Geographic locations

Dynamic maps use data from multiple sources that can update documentation either on schedule or in real time. 

**Note:** If required, you can export these dynamic maps to a static Visio map.


## Runbook Troubleshooting

### Runbook Automation (RBA)
Runbooks are a collection of procedures that we execute from within the Netbrain platform.

eg A route goes dissapears in your network all the suddent, which steps do you take to troubleshoot this incident? Logging information? traceroutes? ping? comparisons between multiple routing tables? While looking at your static visio diagram , all of these and more can be automatically performed by Netbrain's runbook so that you can pinpoint the issue much faster. Again, reducing MTTR.

These runbooks can define the processes we need, and collect information throughout its execution.


## Data view
### Pre-decoded network data
A Data view presents information on our dynamic maps in the form of layers.

Any desired "Data view" can be shown at the map depending on what we need to see. In other diagramming applications, layering the information is either limited or too complicated, eg [MS Visio](http://networkdiagram101.com/?page_id=113)

The best part is that dataviews can be updated automatically. When a change happens on the network, the data is automatically updated. So that we have a more consistent map.

### Single Pane of Glass

Data-views pull information onto the map from multiple data sources:
- Database (Benchmark)
- Parser
- Controller
- API-sourced information

Data can be pulled fromthe network, or third-party apps via RESTful API.

**Note:** Single pane of glass doesn't necessarily means that all the information is displayed on one screen, instead, is an attempt to provide access to all the information from a single screen. (Links, Bookmarks, tabs etc)

## Qapp

### Quick automation application

These applications, are built on top of the "Qapp Automation Framework". They collect data using multiple sources. eg SNMP, CLI, Configuration files, APIs, etc.

The data is analyzed and annotated in different ways.

A few examples are:
- Monitor attributes and create alarms based on thresholds (eg iface; cpu; ram; utilization)
- Highlight interfaces and devices based on a conditional logic (if this, then that)
- Update device attributes on the Netbrain database (plaform details)
- Perform neighbor checks to ensure matching values (eg *K* EIGRP values, FHRP values etc)

### Instant Qapp

A simplified version of a full Qapp, you can drag-n-drop retreieval of data using the parser lib. 

You can setup alerts and color codes to help visualize the data for the task at hand. 

These can be saved into a regular Qapp for further reporting.