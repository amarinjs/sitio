---
layout: article
title: switchport modes
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
tags: trunk access port dynamic desirable mode
---

<!--more-->

---

## Overview
A layer 2 switch port can be configured to operate in either a static mode or a dynamic mode. When operating in static mode, the switch port is explicitly configured as either an access port or a trunk.

An access port is a member of a virtual LAN (VLAN) and is tipically connected to an edge device, like a server or a workstation. A trunk port is not a member of any VLAN and is typically connected to a nonedge device, such as a switch or a router.

By default, an access port on a Cisco switch operates in VLAN 1.

Access ports automatically drop VLAN-tagged frames.

The static switchport modes are also referred to as on and off to indicate wether a port has been statically configured as a trunk port or an access port, respectively.

The off keyword in the mode column of the following sample output from the show interfaces trunk command indicates that the FastEthernet 0/5 interface is configured to operate in a static access mode

    Port        Mode         Encapsulation  Status        Native vlan
    Fa0/18      off          802.1q         trunking      1

### Commands

The *switchport mode access* command will place the switch port into a perma access mode. A port set to acces mode will never become a trunk link.

The *switchport mode trunk* command will place the switch port into perma trunking mode. If the other end of the link is configured as trunk port or is set to dynamic auto or dynamic desirable mode, the link will become a trunk link.

When operating in a dynamic mode, a switch port uses Dynamic Trunking protocol (DTP) to negotiate wether it should operate as an access or trunk port. There are two dynamic modes of operation for a switch port:

- Auto - operates in access mode unless the neighboring interface actively negotiates to operate as a trunk
- Desirable - operates in access mode unless it can actively negotiate a trunk connection witha neighboring interface

The *switchport mode dynamic auto* command configures a port to become a trunk port only if the other link is explicitly configured as a trunk port or is set to dynamic desirable mode. The port will not actively negotiate to become a trunk port

The *switchport mode dynamic desirable* command configures a port to actively negotiate to become a trunk port. The port will send out DTP frames in an attempt to negotiate trunking mode. If the other end of the link is configured as a trunk port or is set to dynamic auto or dynamic desirable mode, the link will become a trunk link. Cisco recommends that you set both sides of a trunk link to dynamic desirable mode when using DTP. When trunking to a non-Cisco device, you should manually configure the switch port for trunk mode.

<left><img src="https://github.com/alexma2344/sitio/blob/master/assets/images/trunk-modes.png?raw=true"></left>

The default dynamic mode is dependent on the hardware platform. In general, departmental-level or wiring closet-level switches default to auto mode, whereas backbone-level switches default to desirable mode. Because a switch port in auto mode does not actively negotiate to operate in trunk mode, it will form a trunk link only if negotiations are initiated by the neighboring interface. A neighboring interface will initiate negotiations only if it is configured to operate in trunk mode or desirable mode. By contrast, a switch port in desirable mode will actively negotiate to operate in trunk mode and will form a trunk link with a neighboring port that is configured to operate in trunk, desirable, or auto mode.

*switchport nonegotiate* command will disable DTP from the port. A port set to nonegotiate mode will not participate in DTP negotiation, nor will the port send DTP frames. Therefore, *switchport nonegotiate* command disables DTP.


Reference: [Configuring VLANs:Trunking](https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst2960/software/release/12-2_55_se/configuration/guide/scg_2960/swvlan.html#pgfId-1096213)
