---
layout: article
title: SSH Keys Linux Access
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
tags: ssh key cryptography
---

<!--more-->

This guide describes how to set up SSH key-based authentication to a Raspberry Pi using Windows PowerShell.  

---

## 1. Generate an SSH Key Pair

Open **Windows PowerShell** and run:

```powershell
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Press Enter to accept the default path (C:\Users\YourName\.ssh\id_ed25519).

Optionally enter a passphrase for extra security.

Display the public key to copy it:

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub
```
Copy the entire output line. It should start with ssh-ed25519.

## 2. Login to Linux Host normally

Create the .ssh Directory on the Pi

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
```

## 3. Create or edit **authorized_keys** File

Open the file

```bash
nano ~/.ssh/authorized_keys
```

In the editor, paste the entire public key

## 4. Set correct permissions

```bash
chmod 600 ~/.ssh/authorized_keys
```

Confirm configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

Ensure the following lines are set:

```bash
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication yes
```
sudo systemctl restart ssh


## 5. Test SSH Key Authentication

```powershell
ssh username@<ip_address>
```

It should connect without a password prompt

## 6. (Optional) Disable Password Authentication


After confirming key-based login works, you can improve security by disabling password login:

```bash
sudo nano /etc/ssh/sshd_config
```

set:
```nginx
PasswordAuthentication no
```

Restart SSH

```bash
sudo systemctl restart ssh
```

### Notes
If key authentication fails, test with verbose output:

```powershell
ssh -vvv username@<ip_address>
```

Ensure correct permissions on the Pi:

```bash
~/.ssh directory: 700
~/.ssh/authorized_keys: 600
```
Do not delete your private key (id_ed25519) on your Windows machine.
