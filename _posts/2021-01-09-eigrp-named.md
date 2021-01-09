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

### restrictions for conversions

- You must use the eigrp upgrade-cli command to convert Enhanced Interior Gateway Routing Protocol (EIGRP) configurations from classic mode to named mode. If multiple classic mode configurations exist, you must use this command per EIGRP autonomous system number in classic mode.

- The eigrp upgrade-cli command blocks the router from accepting any other command until the conversion is complete (the console is locked). The time taken to complete the conversion depends on the size of the configuration. However, the conversion is a one-time activity.
- The eigrp upgrade-cli command is available only under EIGRP classic router configuration mode. Therefore, you can convert configurations from classic mode to named mode but not vice-versa.

- After conversion, the running configuration on the device will show only named mode configurations; you will be unable to see any classic mode configurations. To revert to classic mode configurations, you can reload the router without saving the running configuration to the startup configuration.

- The new configurations are available only in the running configuration; they will not be saved to the startup configuration. If you want to add them to the startup configuration, you must explicitly save them using the write memory or the copy running-config startup-config command.

- After conversion, the copy startup-config running-config command will fail because you cannot have both the classic and named mode for the same autonomous system.

- After conversion, all neighbors (under the converted router EIGRP) will undergo graceful restart and sync all routes.