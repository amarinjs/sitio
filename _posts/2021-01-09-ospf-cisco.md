---
layout: article
title: ospf cisco single-area
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
tags: netbrain tenant
---

<!--more-->

---

## ipv4 ospf cisco

<center><img src="https://github.com/alexma2344/sitio/blob/master/assets/images/ospf-ref-topo.jpg?raw=true"></center>
<div style="text-align: center;">
    <span style="font-size:11px; color:grey">
        ospf reference topology.
    </span>
</div>


### ospf configuration

	router ospf <process-id>
		network <subnet> <wildcard-mask> <area number>

*The process ID doesn't need to match for adjacencies to be formed.*


[**show commands**](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/command/iro-cr-book/ospf-s1.html)


