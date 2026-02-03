---
title: "Setting Up Linux Mint as a Home Server"
date: 2026-02-02
description: "How to configure Linux Mint XFCE as a lightweight, always-on home server for self-hosting and passive income apps."
tags: ["tutorial", "linux", "home-server"]
categories: ["Tutorials"]
keywords: ["Linux Mint server", "home server setup", "XFCE server", "lightweight Linux server"]
ShowToc: true
TocOpen: true
---

Linux Mint XFCE is an excellent choice for a home server. It's lightweight, stable, and easy to manage. Here's how to set it up.

## Why Linux Mint XFCE?

- **Low resource usage** - XFCE uses minimal RAM and CPU
- **Stable** - Based on Ubuntu LTS, very reliable
- **User-friendly** - Easy to manage if you need a GUI
- **Great hardware support** - Works on most machines out of the box

## Installation

1. Download Linux Mint XFCE from the official website
2. Create a bootable USB with Rufus or Balena Etcher
3. Boot from USB and install
4. Choose "Install third-party software" for better hardware support

## Post-Install Configuration

### Update Everything

```bash
sudo apt update && sudo apt upgrade -y
```

### Install Essential Tools

```bash
sudo apt install -y \
  htop \
  curl \
  wget \
  git \
  docker.io \
  docker-compose \
  net-tools \
  openssh-server
```

### Enable SSH

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

Now you can manage the server remotely.

### Configure Static IP

For a server, you want a static IP. Edit your connection:

1. Right-click network icon → Edit Connections
2. Select your connection → IPv4 Settings
3. Change Method to "Manual"
4. Add your IP, netmask, gateway, DNS

Or via command line:

```bash
sudo nano /etc/netplan/01-network-manager-all.yaml
```

### Set Up Auto-Login (Headless Operation)

Edit LightDM config:

```bash
sudo nano /etc/lightdm/lightdm.conf
```

Add under `[Seat:*]`:

```
autologin-user=yourusername
autologin-user-timeout=0
```

### Disable Screen Blanking

```bash
xset s off
xset -dpms
xset s noblank
```

Add these to your `.bashrc` or startup scripts.

### Configure Firewall

```bash
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

Add any other ports your apps need.

## Docker Setup

Docker makes running services easy:

```bash
# Add yourself to docker group
sudo usermod -aG docker $USER

# Log out and back in, then verify
docker run hello-world
```

## Monitoring

### Check System Resources

```bash
htop
```

### Check Disk Usage

```bash
df -h
```

### Check Running Services

```bash
sudo systemctl list-units --type=service --state=running
```

## Automatic Updates (Optional)

For security updates:

```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```

## Power Management

Disable sleep/suspend for 24/7 operation:

```bash
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

## Remote Access Options

### SSH (Command Line)

From another computer:

```bash
ssh username@server-ip
```

### VNC (Graphical)

Install x11vnc for remote desktop access:

```bash
sudo apt install x11vnc
x11vnc -display :0 -forever -bg -o /tmp/x11vnc.log
```

## Maintenance

### Weekly Tasks

- Check disk space: `df -h`
- Review logs: `journalctl -p err --since "1 week ago"`
- Update system: `sudo apt update && sudo apt upgrade`

### Monthly Tasks

- Reboot to apply kernel updates
- Clean old packages: `sudo apt autoremove`
- Check SMART status of drives: `sudo smartctl -a /dev/sda`

## Conclusion

Linux Mint XFCE provides a solid, low-maintenance foundation for a home server. Once configured, it can run for months without intervention.

The key is initial setup - get SSH, Docker, and auto-login configured, and you can manage everything remotely.
