---
layout: article
title: Error Detection
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(34, 139, 87 , .7), rgba(139, 34, 139, .5))'
    src: https://github.com/alexma2344/peperina/blob/master/assets/images/radiohead.jpg?raw=true"
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

eg Making a wire transfer and having bits flipped in transit. Could have a big impact on the outcome of the operation.

These are some error detection algorithms used to address the above.
- Checksums
- Cyclic redundancy codes (CRCs)
- Message authentication codes (MACs)

## Checksum

## CRC

## MAC