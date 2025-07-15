---
layout: article
title: NIST Guide
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
tags: NIST compliance
---

<!--more-->

# Cisco IOS Security Configuration Guide

This guide provides key configuration recommendations for securing Cisco IOS devices, aligned with the NIST Cybersecurity Framework (CSF) and based on the Center for Internet Security (CIS) benchmarks. These configurations help network administrators enhance the security posture of their Cisco routers and switches.

## Configuration Recommendations

| NIST CSF Subcategory | Description | CIS Benchmark Recommendation | Configuration Command |
|---------------------|-------------|-----------------------------|-----------------------|
| PR.AA-01 | Identities and credentials are managed and verified for authorized users and devices. | Enable AAA | Centralizes authentication, authorization, and accounting for better control. | `aaa new-model` |
| PR.AA-01 | Identities and credentials are managed and verified for authorized users and devices. | Set up AAA authentication for login | Uses local database or external server for user authentication. | `aaa authentication login default local` |
| PR.AA-02 | Privileged access is managed and restricted. | Set up AAA authentication for enable mode | Controls access to privileged EXEC mode. | `aaa authentication enable default enable` |
| PR.AA-01 | Identities and credentials are managed and verified for authorized users and devices. | Create local user with encrypted password | Ensures secure local authentication if external servers are unavailable. | `username <name> secret <password>` |
| PR.DS-05 | Protections against data leaks are implemented. | Enable SSH | Provides secure remote access to the device, preventing data exposure. | `ip ssh version 2`<br>`crypto key generate rsa modulus 2048` |
| PR.DS-05 | Protections against data leaks are implemented. | Set VTY lines to use SSH only | Disables insecure protocols like Telnet for remote access. | `line vty 0 4`<br>`transport input ssh` |
| PR.AA-05 | Session lock and termination policies are enforced. | Set session timeout | Automatically logs out inactive sessions to prevent unauthorized access. | `line con 0`<br>`exec-timeout 10 0`<br>`line vty 0 4`<br>`exec-timeout 10 0` |
| DE.CM-01 | Networks are monitored to detect adverse events. | Enable logging | Captures system events for monitoring and auditing purposes. | `logging on`<br>`logging host <syslog_server_ip>` |
| DE.CM-01 | Networks are monitored to detect adverse events. | Set logging level | Configures the severity level of logs to capture necessary events. | `logging trap informational` |
| PR.PS-01 | Configuration management practices are applied. | Secure SNMP (if used) | Uses secure community strings or preferably SNMPv3 for management. | `snmp-server community <string> RO`<br>(Consider using SNMPv3 for better security) |
| PR.PS-01 | Configuration management practices are applied. | Require Unicast Reverse-Path Forwarding | Verifies source addresses to prevent IP spoofing. | `ip cef`<br>`interface <interface_name>`<br>`ip verify unicast source reachable-via rx` |
| PR.PS-01 | Configuration management practices are applied. | Forbid IP Proxy ARP | Disables proxy ARP to maintain LAN security boundaries. | `interface <interface_name>`<br>`no ip proxy-arp` |

## Notes
- **Command Variations**: The exact commands may vary slightly depending on the Cisco IOS version (e.g., IOS 15.x, 16.x, or 17.x). Always refer to the specific CIS benchmark document for your IOS version for precise guidance.
- **CIS Benchmarks Access**: The recommendations are based on CIS benchmarks for Cisco IOS, available at [CIS Cisco Benchmarks](https://www.cisecurity.org/benchmark/cisco). A CIS SecureSuite membership may be required for full access.
- **Implementation Considerations**: Some configurations, like SNMPv3, require additional setup (e.g., user authentication and encryption settings). Ensure compatibility with your network environment before applying changes.
- **Verification Tools**: Tools like CIS-CAT Pro or open-source scripts (e.g., [ONYX tool](https://github.com/UncleSocks/onyx-caaat-automated-cisco-ios-configuration-assessment-and-auditing-tool)) can automate compliance checks against CIS benchmarks.