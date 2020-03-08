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
tags: computer-networks routing ospf
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

So it sends **HELLO** packets on each of its lines. The router on the other end is expected to reply giving its name.

When we have multiple routers on a single broadcast domain *Figure 5-11(a)*, it leads to wasteful messages.

<img src="https://github.com/alexma2344/sitio/blob/master/assets/images/fig-5-11.jpg?raw=true">

So instead we consider the LAN as a node itself: **N** *Figure 5-11(b)*, then one "designated router" on that LAN is selected to play the role of **N**. So going from A to C will be ANC.


### Setting link costs

Link state requires each link to have a cost metric (distance), so it can find the shortest path.

#### Bandwidth

A common choice is to make the **cost** inversely proportional to the bandwidth of the link. So that higher capacity paths are better choices.

**eg** 1Gbps Ethernet may have a cost of **1**, then 100Mbps Eth a cost of **10**. 

#### Delay

Then for a geographically spread out network, **Delay** is factored into the cost, so that shorter links are better choices.

To determine the delay an **ECHO** packet is sent over the line, the other side replies immediately. By meassuring the round-trip time and dividing it by two, we get an estimated delay. 

If we ping from London to Buenos Aires the round trip time is 245 miliseconds, divided by two is 122 miliseconds. Quick maths.

<img src="https://github.com/alexma2344/sitio/blob/master/assets/images/rtt-average.jpg?raw=true">

### Building link state packets

Once the information needed for the exchange has been collected, each router will build a packet containing all the data.

Packet contains:

- Identity of sender 
- Sequence number
- Age  
- List of neighbors  

<img src="https://github.com/alexma2344/sitio/blob/master/assets/images/lsa.jpg?raw=true">


[OSPF packet capture](https://www.cloudshark.org/captures/293956261434)


### Distributing link state packets

All routers must get all state packets quickly and reliably. If different routers have different versions of the topology, inconsistencies such as loops or unreachable destinations.

#### Flooding

We use flooding to distribute these packets, as a control meassure each packet contains the sequence number (32-bit) that is incremented for each new packet sent.

> With one link state packet per second, it would take 137 years to wrap around, so this possibility can be ignored.

When a new packet comes in, it is checked against the list of packets that arrived earlier, if new, then it is flooded again.
If is a duplicate then it is discarded.

### Computing the New Routes

When the router has accumulated a full set of link state packets, it can construct the entire network graph, because all links are represented. (Actually links are represented twice, once for each direction, and each direction may have different costs).

Compared to distance vector routing, link state routing requires more memory
and computation.


