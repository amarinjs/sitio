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

The idea behind link state routing is fairly simple and can be stated as five
parts. Each router must do the following things to make it work:

1. Discover neighbors and learn their network addresses.
2. Set the distance or cost metric to each of its neighbors.
3. Construct a packet telling all it has learned on the previous steps.
4. Send this packet to and receive similar packets from all other routers.
5. Compute shortest path to every other router.

### Learning about neighbors

When a router is booted its first task is to learn who its neighbors are.

So it sends **'HELLO'** packets on each of its lines. The router on the other end is expected to reply giving its name.

When we have multiple routers on a single broadcast domain *Figure 5-11(a)*, it leads to wasteful messages.

<img src="https://github.com/alexma2344/sitio/blob/master/assets/images/fig-5-11.jpg?raw=true">

So instead we consider the LAN as a node itself *Figure 5-11(b)*