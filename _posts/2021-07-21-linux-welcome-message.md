---
title: "Custom Linux Login Banners (MOTD, SSH, and ASCII Art)"
date: 2021-07-21
description: "Learn how to customize Linux login banners using /etc/issue.net, /etc/motd, and SSH configuration. Includes persistent setup for cloud environments and fun ASCII art banners with figlet and toilet."
author: Prince Praveen
---

## 🧩 Customizing Linux Login Banners

Bored of the default login banners? Let’s customize the welcome message on most Linux systems.

Login messages are displayed in two stages:

1. **Before authentication (SSH login prompt)**
2. **After login (MOTD - Message of the Day)**

---

## 🔐 1. Before Login (Pre-Authentication Banner)

Edit:

```bash
sudo nano /etc/issue.net
```

👉 To enable this banner via SSH, update your SSH configuration (see below).

📘 Reference: [OpenSSH sshd_config documentation](https://man.openbsd.org/sshd_config)

---

## 🔓 2. After Login (Post-Authentication Banner)

Edit the MOTD file:

```bash
sudo nano /etc/motd
```

📘 Reference: [Linux MOTD documentation](https://man7.org/linux/man-pages/man5/motd.5.html)

---

## 📝 Example Security Banner

```text
This system is for authorized use only. All activities are logged and checked at frequent intervals. Unauthorized individuals attempting to connect to, port-scan, deface, hack, or otherwise interfere with any services on this system will be reported.
```

---

## ⚠️ Cloud Caveat (AWS, Azure, etc.)

On cloud platforms like:

- [AWS EC2](https://aws.amazon.com/ec2/)
- [Microsoft Azure VM](https://azure.microsoft.com/en-in/products/virtual-machines/)

Your changes may **reset after reboot** due to dynamic MOTD scripts.

---

## 🛠️ Make Your Banner Persistent

### Option 1: Disable MOTD Update Script

```bash
sudo update-motd --disable
```

> ⚠️ This may not exist on all distributions. On Ubuntu, dynamic scripts live under `/etc/update-motd.d/`.

📘 Reference: [Ubuntu Dynamic MOTD](https://ubuntu.com/server/docs/login-welcome-message)

---

### Option 2: Configure SSH Banner Properly

Edit SSH config:

```bash
sudo nano /etc/ssh/sshd_config
```

Find:

```bash
#Banner /etc/issue.net
```

Uncomment it:

```bash
Banner /etc/issue.net
```

Restart SSH:

```bash
sudo systemctl restart sshd
```

---

## 🎨 Bonus: ASCII Art Banners

Want something more fun and personalized? Use ASCII generators.

### 🔧 Tools

- [FIGlet](http://www.figlet.org/) — simple CLI tool for ASCII text
- [Toilet](https://caca.zoy.org/wiki/toilet) — adds styling and colors

---

## 📦 Installation

### Debian / Ubuntu

```bash
sudo apt install figlet toilet
```

### CentOS / RHEL

```bash
sudo yum install figlet toilet
```

### Fedora

```bash
sudo dnf install figlet toilet
```

---

## 🚀 Example Usage

```bash
figlet DevOps
```

Try styles:

```bash
toilet -f mono12 -F metal DevOps
```

Example output:

```text
     _           
    | | ___  ___ _   _ ___  
 _  | |/ _ \/ __| | | / __| 
| |_| |  __/\__ \ |_| \__ \ 
 \___/ \___||___/\__,_|___/ 
```

---

## 🔗 Related Posts

- 👉 [Linux Kernel Performance Tuning](/linux-kernel-performance-tuning)
- 👉 [SRE Metrics That Matter](/sre-metrics-that-matter)
- 👉 [Infrastructure as Code Patterns](/infrastructure-as-code-patterns)

---

## 💡 Pro Tips

- Combine **security warning + ASCII branding**
- Keep banners short for production systems
- Use `/etc/issue.net` only for **legal/security notices**
- Avoid leaking system info (security risk)

---

## ✅ Wrap Up

Custom login banners are a small tweak—but they:

- Improve security posture  
- Add professional branding  
- Enhance developer experience  

---

💬 *Have a cool banner setup? Share it!*

---

[🚀 **Visit me for more DevOps content**](https://www.princepraveen.co.in)