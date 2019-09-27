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
mathjax: true
mathjax_autoNumber: true
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

	f(Data) = ED

Whereas IP prepends a checksum on its header.

	ED = f(Data)


## Checksum

It adds up all the values in the packet (IP,TCP)

<a class="button button--primary button--rounded button--xs" href="">PROS</a> Fast and cheap even in software

<a class="button button--primary button--rounded button--xs" href="">CONS</a> Not very robust


## CRC

<a class="button button--primary button--rounded button--xs" href="">PROS</a> More robust, protects against any 2 bit error

<a class="button button--primary button--rounded button--xs" href="">CONS</a> More expensive than checksum (easy in HW)

## MAC

<a class="button button--primary button--rounded button--xs" href="">PROS</a> Robust to malicious modifications

<a class="button button--primary button--rounded button--xs" href="">CONS</a> Not so robust to errors