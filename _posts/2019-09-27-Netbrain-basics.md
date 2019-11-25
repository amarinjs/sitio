---
layout: article
title: NetBrain Essentials
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#4c4cc2'
  background_image:
    gradient: 'linear-gradient(313deg, rgba(2,0,36, .3) 0%, rgba(76,76,194, .3) 47%, rgba(0,212,255, .6) 100%)'
    src: https://github.com/alexma2344/sitio/blob/master/assets/images/brain.jpg?raw=true"
sharing: true
comment: true
articles:
  excerpt_type: html
tags: computer-networks netbrain
---

<!--more-->

---

# NetBrain Essentials

Netbrain is a network [automation](https://en.wikipedia.org/wiki/Automation#targetText=Automation%20is%20the%20technology%20by,performed%20with%20minimal%20human%20assistance.) [platform](https://en.wikipedia.org/wiki/Computing_platform). That brings dynamic maps and runbooks to our day to day IT operations.

With this platform you can reduce metrics such as:
- Mean time to repair (MTTR)
- Mean time between failures (MTBF)
- Mean time to contain or mitigate a security incident
- Automating the provisioning of IT resources


## Dynamic Mapping

A dynamic [map](https://en.wikipedia.org/wiki/Map) is an interactive depiction, that emphasizes the relationship between **elements** on a **space**. 

Dynamic maps use data from multiple sources that can update documentation either on schedule or in real time. 

In Nebrain, every network task begins with a data driven dynamic map.

<center><img src="https://github.com/alexma2344/sitio/blob/master/assets/images/dmap1.PNG?raw=true"></center>
<div style="text-align: center;">
    <span style="font-size:11px; color:grey">
        example of an A to B path task. 
    </span>
</div>


## Data Views
### Pre-decoded network data
A Data view presents information that helps us understand the design of real newtworks.

It can integrate with third party systems and can become the single pane of glass for your network.

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


## Runbook Troubleshooting

### Runbook Automation (RBA)
Runbooks are the series of steps that we take to troubleshoot an issue. In Netbrain, we document our runbooks to **standarize** our approach to incident solving.

eg A service on the network becomes unreachable, which steps do you take to troubleshoot this incident? Logging information? traceroutes? ping? comparisons between multiple routing tables? using netbrain all of these take seconds and are much more precise than a human operator.

These runbooks can define the processes we need, pluse they collects meaningful information throughout its execution.

## Qapp

### Quick Applications

These applications collect data using multiple sources. eg SNMP, CLI, Configuration files, APIs, etc.

The data is analyzed and annotated on a more didactic way.

A few examples are:
- Monitor attributes and create alarms based on thresholds (eg iface; cpu; ram; utilization)
- Highlight interfaces and devices based on a conditional logic (if this, then that)
- Update device attributes on the Netbrain database (plaform details)
- Perform neighbor checks to ensure matching values (eg *K* EIGRP values, keepalives, FHRP values etc)

### Instant Qapp

With instant Qapps we can visualize [parser](https://en.wikipedia.org/wiki/Parsing) variables on a map

<center><img src="https://github.com/alexma2344/sitio/blob/master/assets/images/iqappret.png?raw=true"></center>
<div style="text-align: center;">
    <span style="font-size:11px; color:grey">
        Instant Qapp parsing data on our map. 
    </span>
</div>

You can setup alerts and color codes to help visualize the data for the task you're performing.

These can be saved into a regular Qapp for further reporting.