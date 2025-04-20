---
layout: article
title: Access Control cheat-sheet
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
tags: cisco ios-xe acs radius tacacs access-control aaa security
---

<!--more-->

---

# AAA in Network Security

AAA is a foundational security framework used in networking to manage user access and enforce policies. It stands for:

**Authentication** – verifying a user’s identity.

**Authorization** – determining what resources a user is allowed to access.

**Accounting** – logging user activity for audit and billing purposes.


## Origins and Purpose

AAA emerged from the need to secure increasingly complex and distributed networks. In early systems, access control was limited to simple passwords. As organizations adopted remote access, VPNs, and multi-user devices, a scalable and flexible access control method became necessary. AAA was formalized by protocols like RADIUS and TACACS+, and became a standard in enterprise-grade network infrastructure.

### How it Works

AAA operates by decoupling access control from the device itself, enabling centralized control via an external server (such as a RADIUS or TACACS+ server). Here’s a high-level flow:

Authentication: When a user connects to a network device (via SSH, console, etc.), the device checks their credentials against an external source (e.g., RADIUS) or local database.

Authorization: Once authenticated, the server responds with a list of allowed privileges (e.g., can enter enable mode, execute certain commands).

Accounting: The device records the session details (login time, commands run, logout time) and sends them to the accounting server.

This approach allows network admins to centrally manage user permissions, enforce policy, and maintain logs for compliance and troubleshooting.

### Configuration:

	aaa new-model >> control is handled via AAA policies and external servers
	radius-server host 192.0.2.1 key MySecretKey >> Define and point to ACS
	aaa authentication login default group radius local >> 
	aaa authorization exec default group radius local
	aaa accounting exec default start-stop group radius


### Command Breakdown: `aaa authentication login default group radius local`

This command defines how users are authenticated when logging in (e.g., via console or VTY) using the `default` method list. It attempts to authenticate via RADIUS first, and if that fails, falls back to the local user database.

---

### 🔍 Keyword-by-Keyword Breakdown

| **Keyword**       | **Meaning**                                                                 |
|-------------------|------------------------------------------------------------------------------|
| `aaa`             | Begins an AAA-related configuration line                                     |
| `authentication`  | Specifies the configuration is for authentication (who the user is)         |
| `login`           | Applies this method list to login events (e.g., console, VTY, SSH access)   |
| `default`         | Name of the method list; `default` applies to all lines unless overridden   |
| `group radius`    | First tries authentication via the configured RADIUS server(s)              |
| `local`           | If RADIUS fails or is unreachable, fallback to the local user database      |

---

### ⚙️ Available Options

#### After `aaa authentication`:
- `login` – For user login (VTY, console)
- `enable` – For enable mode access
- `dot1x` – For 802.1X port authentication
- `ppp` – For PPP session authentication
- `arap` – For AppleTalk Remote Access Protocol

#### For `default`:
- You can specify any custom method list name (e.g., `vpn-users`, `console-auth`).
- `default` is a **reserved keyword** that applies to all interfaces unless a specific method list is applied.

#### After `group`:
- `radius` – Use RADIUS servers
- `tacacs+` – Use TACACS+ servers
- Custom-defined server groups can also be used

#### Final methods (fallback options):
- `local` – Use local user database (`username` commands)
- `line` – Use password defined under `line vty` or `line console`
- `none` – No authentication (⚠️ **not recommended**)

---

### 💡 Example Configuration

```bash
username admin privilege 15 secret StrongPass123

aaa new-model
radius-server host 192.0.2.10 key RadiusSecret
aaa authentication login default group radius local

line vty 0 4
 login authentication default

