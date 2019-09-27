---
layout: article
title: Error Detection
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#4c4cc2'
  background_image:
    gradient: 'linear-gradient(313deg, rgba(2,0,36, .3) 0%, rgba(76,76,194, .3) 47%, rgba(0,212,255, .6) 100%)'
    src: https://github.com/alexma2344/peperina/blob/master/assets/images/don-ripper.jpg?raw=true"
sharing: true
comment: true
articles:
  excerpt_type: html
tags: computer-networks
---

<!--more-->

---

## Intro

The chances of errors on the transmissions over the network are always latent.

Imagine that the packets we send and receive usually have to go through our local network, to an ISP maintained physical medium, trough different jumps on internet nodes, and to the server located on a remote infrastructure.

**eg** Making a wire transfer and having bits flipped in transit.

Could have a big impact on the outcome of the operation.

The following are some error detection (ED) algorithms used to address the above. Each of them very differnt from the other.
- Checksums
- Cyclic redundancy codes (CRCs)
- Message authentication codes (MACs)

Ethernet and TLS appends ED to the payload. 

<div class="grid">
  <div class="cell cell--8">Data</div>
  <div class="cell cell--auto">ED</div>
</div>


Whereas IP prepends a checksum on its header.

<div class="grid">
  <div class="cell cell--8">Data</div>
  <div class="cell cell--auto">ED</div>
</div>



## Checksum

## CRC

## MAC