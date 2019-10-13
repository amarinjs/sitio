---
layout: article
title: Netbrain Sites + MPLS definition
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#4c4cc2'
  background_image:
    gradient: 'linear-gradient(313deg, rgba(2,0,36, .3) 0%, rgba(76,76,194, .3) 47%, rgba(0,212,255, .6) 100%)'
    src: https://github.com/alexma2344/peperina/blob/master/assets/images/brain.jpg?raw=true"
sharing: true
comment: true
articles:
  excerpt_type: html
tags: netbrain MPLS
---

<!--more-->

---

## Sites

Sites in Netbrain allows us to scope our network on a geographical basis.

The two types of sites are:
- Container Site (parent), can contain leaf sites and other containers, and cannot nest devices if not through a leaf.
- Leaf Site (child), contains devices. That can be added manually or dynamically

What's cool about this, is that we can automatically add devices to sites based on conditionals. eg the begining letters of the hostname, that would indicate the city ie LND, SHE, WOR etc

## MPLS Clouds

The Netbrain system introduces "MPLS Clouds" to simulate MPLS functions, these are taken as devices by the system, and they have virtual routing tables to allow A to B Path calculations.

<center><img src="https://github.com/alexma2344/peperina/blob/master/assets/images/mpls_cloud_structure.png?raw=true"></center>
<div style="text-align: center;">
    <span style="font-size:11px; color:grey">
        structure of an mpls cloud.
    </span>
</div>

