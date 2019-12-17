---
layout: article
title: Multi-Tenancy
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
mathjax: true
mathjax_autoNumber: true
articles:
  excerpt_type: html
tags: netbrain tenant
---

<!--more-->

---


# Admin Framework


<center><img src="https://github.com/alexma2344/sitio/blob/master/assets/images/tenant.PNG?raw=true"></center>
<div style="text-align: center;">
    <span style="font-size:11px; color:grey">
        Network device planes. 
    </span>
</div>


There are three structural levels to a NetBrain IE deployment:

- System
- Tenant
- Domain


The **system** level is the deployment as a whole. Each **Tenant** has an entirely separate database (for data segregation). And each **Domain** contains different topologies, which can be applied to separate parts of the same network.


### Purpose

This per tenant framework was made having MSPs in mind. So that they can manage multiple customers, each of which will belong to a unique database.