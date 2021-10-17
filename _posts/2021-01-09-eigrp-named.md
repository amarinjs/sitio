---
layout: article
title: eigrp named mode
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
tags: eigrp routing computer-networks
---

<!--more-->

---

### benefits

eigrp named mode is easier to configure than classic mode. if using ipv6 for example there's no need to configure individual interfaces.

configuration:

    router eigrp <word>
      address-family <ipv4|ipv6> autonomous-system <number> 

with the address family command, the named instance can form adjacencies with neighbors on an AS.

run the network statements as usual within that address family.


### functionality

on the protocol side nothing changes, seems like an ease of configuration upgrade.


### classic to named mode conversion

    eigrp upgrade-cli

notes on this conversion:

[Cisco Link](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_eigrp/configuration/xe-3s/ire-xe-3s-book/ire-classic-to-named.html#:~:text=The%20eigrp%20upgrade%2Dcli%20command,is%20a%20one%2Dtime%20activity.)