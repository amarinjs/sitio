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

eigrp named mode is easier to configure than classic mode. if using ipv6 for example you don't need to configure individual interfaces.

configuration:

    router eigrp <word>
      address-family <ipv4|ipv6> autonomous-system <number> 

With the address family command, our named instance can form adjacencies with neighbors on an AS.

The network statements are as usual.