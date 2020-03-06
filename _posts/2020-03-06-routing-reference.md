---
layout: article
title: Routing Reference
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

# Routing Algorithms

A part of the network layer software responsible for deciding the output line for a packet.

Make the distinction between **routing**, which is making the decision which routes to use. 
And **forwarding**, which is what happens when a packet arrives.

Routing algorithms can be grouped into two clases:

- Nonadaptive algorithms (static routing)

- Adaptive algorithms (dynamic routing)

## Link State

> Distance vector was used in ARPANET untill 1979, then replaced by link state.
> A main reason was that distance vector took too long to convert after a topology change (count to infinity problem).