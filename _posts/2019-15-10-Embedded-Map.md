---
layout: article
title: Embedded Map
mode: immersive
header:
  theme: dark
article_header:
  type: overlay
  theme: dark
  background_color: '#4c4cc2'
  background_image:
    gradient: 'linear-gradient(313deg, rgba(2,0,36, .3) 0%, rgba(76,76,194, .3) 47%, rgba(0,212,255, .6) 100%)'
    src: https://github.com/alexma2344/peperina/blob/master/assets/images/brain.jpg?raw=true"
sharing: true
comment: true
articles:
  excerpt_type: html
tags: netbrain
---

<!--more-->

---

Netbrain supports integration with web portals

**Note:** iframe size should be 1200 x 800

### Process

Open the **web.config** file from the following location:

	C:\Program Files\NetBrain\Web Server\nb_publish_server\

Search for a section that looks like this

```html
    <httpProtocol>
      <customHeaders>
        <remove name="X-Powered-By" />
        <remove name="X-Frame-Options" />
        <remove name="X-Content-Type-Options" />
        <remove name="X-XSS-Protection" />
        <!--<remove name="Content-Security-Policy"/>-->
        <remove name="Strict-Transport-Security" />
        <add name="X-Frame-Options" value="SAMEORIGIN" />
        <add name="X-Content-Type-Options" value="nosniff" />
        <add name="X-XSS-Protection" value="1; mode=block" />
        <!--<add name="Content-Security-Policy" value="default-src 'self'"/>-->
        <add name="Strict-Transport-Security" value="max-age=31536000; includeSubDomains; preload" />
      </customHeaders>
    </httpProtocol>
```