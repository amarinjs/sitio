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

