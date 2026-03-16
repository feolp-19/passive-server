---
title: "How to Set Up a Linux Mint Home Server From Scratch (Complete Guide)"
date: 2026-04-05T09:00:00+01:00
description: "Step-by-step guide to turning any old computer into a Linux Mint home server. From installation to Docker, SSH, and running it 24/7."
slug: "linux-mint-home-server-setup"
tags: ["guide", "linux", "home-server", "docker"]
categories: ["Guides", "Linux"]
keywords: ["Linux Mint home server", "home server setup 2026", "Linux Mint XFCE server", "old computer server", "Docker home server", "SSH setup Linux", "home server for beginners"]
ShowToc: true
TocOpen: true
faq:
  - question: "Can I use an old computer as a home server?"
    answer: "Yes. Any computer with at least 2GB RAM and a 64-bit processor can run Linux Mint as a home server. Old laptops, desktops, and Mac Minis work great."
  - question: "Why Linux Mint XFCE for a server?"
    answer: "XFCE is the lightest desktop environment, leaving more RAM and CPU for your applications. Linux Mint is stable, beginner-friendly, and based on Ubuntu LTS."
  - question: "Does a home server use a lot of electricity?"
    answer: "No. A Mac Mini or old laptop uses 10-30 watts, costing roughly $3-8 per month in electricity."
  - question: "Do I need a monitor connected to the server?"
    answer: "Only for the initial setup. After configuring SSH, you manage everything remotely from another computer."
---

You don't need a Raspberry Pi. You don't need to buy anything.

That old laptop, desktop, or Mac Mini collecting dust? It's your next home server.

This is the exact setup I used to build my [passive income machine](/old-mac-mini-passive-income-machine/). Follow along.

---

## What You'll Need

- **Any old computer** — Minimum 2 GB RAM, 64-bit processor
- **USB flash drive** — At least 4 GB (for the installer)
- **Internet connection** — Ethernet cable recommended
- **Another computer** — To download the installer and manage via SSH later
- **30-60 minutes** of time

---

## Step 1: Download Linux Mint XFCE

Go to [linuxmint.com/download](https://www.linuxmint.com/download.php) and download the **XFCE edition**.

### Why XFCE?

| Desktop | RAM Usage (Idle) | Best For |
|---------|-----------------|----------|
| Cinnamon | ~750 MB | Daily driver desktop |
| MATE | ~500 MB | Moderate use |
| **XFCE** | **~350 MB** | **Servers and old hardware** |

XFCE leaves the most resources for your applications. On a server, every MB of RAM matters.

---

## Step 2: Create a Bootable USB

Download **Balena Etcher** (free) from [etcher.balena.io](https://etcher.balena.io/).

1. Insert your USB flash drive
2. Open Etcher
3. Select the Linux Mint ISO
4. Select your USB drive
5. Click Flash

---

## Step 3: Install Linux Mint

1. Plug the USB into your server computer
2. Boot from USB (usually F12 or Option key on Mac)
3. Select "Install Linux Mint"
4. Choose your language and keyboard
5. **Check** "Install multimedia codecs"
6. Select **"Erase disk and install"**
7. Set your username and password
8. Wait for installation to complete
9. Reboot and remove the USB

You now have a working Linux Mint desktop.

---

## Step 4: Initial Configuration

Open a terminal (Ctrl+Alt+T) and run:

```bash
sudo apt update && sudo apt upgrade -y
```

### Set the Hostname

Give your server a name:

```bash
sudo hostnamectl set-hostname my-server
```

### Disable Screen Lock and Sleep

A server should never sleep:

1. Open **Settings** → **Power Manager**
2. Set display sleep to **Never**
3. Set system sleep to **Never**
4. Disable screen lock

Or via terminal:

```bash
xfconf-query -c xfce4-power-manager -p /xfce4-power-manager/dpms-enabled -s false
xfconf-query -c xfce4-power-manager -p /xfce4-power-manager/blank-on-ac -s 0
```

---

## Step 5: Install SSH

SSH lets you manage the server from another computer — no monitor needed.

```bash
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
```

### Secure SSH (Optional but Recommended)

Change the default port from 22 to something less common:

```bash
sudo nano /etc/ssh/sshd_config
```

Find the line `#Port 22` and change it to:

```
Port 2255
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

### Connect from Another Computer

Find your server's IP:

```bash
hostname -I
```

From your laptop:

```bash
ssh -p 2255 your-username@192.168.x.x
```

If it connects, you can unplug the monitor from the server.

---

## Step 6: Install Docker

Docker lets you run applications in isolated containers. No messy installations, easy updates, easy removal.

```bash
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker
```

Add your user to the Docker group (so you don't need `sudo` every time):

```bash
sudo usermod -aG docker $USER
```

Log out and back in for this to take effect.

### Verify Docker Works

```bash
docker run hello-world
```

You should see "Hello from Docker!" — you're ready to run containers.

---

## Step 7: Install Watchtower (Auto-Updates)

Watchtower automatically updates your Docker containers when new versions are released:

```bash
docker run -d \
  --restart=unless-stopped \
  --name watchtower \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --cleanup --interval 86400
```

This checks for updates every 24 hours and cleans up old images.

---

## Step 8: Set a Static IP

Prevent your server's IP from changing:

1. Log into your router (usually `192.168.0.1` or `192.168.1.1`)
2. Find **DHCP Reservation** or **Address Reservation**
3. Add your server's MAC address with a fixed IP

This ensures SSH always works at the same address.

---

## Step 9: Enable Automatic Security Updates

```bash
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure -plow unattended-upgrades
```

Select "Yes" to enable automatic security updates. Your server stays patched without manual intervention.

---

## Step 10: Set Up Remote Access (Tailscale)

If you want to reach your server from outside your home network:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up --ssh
```

Follow the authentication link, and you can SSH from anywhere in the world.

---

## What to Run on Your Server

Now that your server is ready, here's what I run on mine:

| Application | Purpose | Guide |
|-------------|---------|-------|
| [Grass](/grass-passive-income-guide/) | Bandwidth sharing | [Setup guide](/grass-passive-income-guide/) |
| [Honeygain](/honeygain-docker-setup-guide/) | Bandwidth sharing | [Setup guide](/honeygain-docker-setup-guide/) |
| [EarnFM](/earnfm-docker-setup-guide/) | Bandwidth sharing | [Setup guide](/earnfm-docker-setup-guide/) |
| [Traffmonetizer](/traffmonetizer-docker-setup-guide/) | Bandwidth sharing | [Setup guide](/traffmonetizer-docker-setup-guide/) |
| [Pawns.app](/pawns-app-setup-guide/) | Bandwidth sharing | [Setup guide](/pawns-app-setup-guide/) |
| [Storj](/storj-node-setup-guide/) | Decentralized storage | [Setup guide](/storj-node-setup-guide/) |

Check out all the [apps I'm running](/apps/) with signup links.

---

## Power Consumption

A common concern: "Won't this cost a fortune in electricity?"

No.

| Hardware | Watts | Monthly Cost (~$0.15/kWh) |
|----------|-------|---------------------------|
| Old laptop | 15-25W | $1.60-2.70 |
| Mac Mini | 10-20W | $1.10-2.20 |
| Desktop PC | 30-80W | $3.25-8.60 |
| Raspberry Pi | 3-5W | $0.30-0.55 |

My Mac Mini costs roughly **$3-5/month** in electricity. The passive income apps earn more than that.

---

## Troubleshooting

### Can't SSH after reboot
Check if SSH is running: `sudo systemctl status ssh`

### Docker command not found after install
Log out and back in, or run: `newgrp docker`

### Server gets a new IP after reboot
Set a static IP via your router's DHCP reservation (Step 8).

### External drive not mounting on boot
Add it to `/etc/fstab` with the `nofail` option. See my [Storj guide](/storj-node-setup-guide/) for details.

---

## Conclusion

You now have a fully functional Linux home server running Docker, accessible via SSH from anywhere.

Total cost: **$0** (assuming you have old hardware).

The hard part is done. From here, you just add Docker containers for whatever you want to run — passive income apps, media servers, home automation, or anything else.

---

*Got your server running? Check out my [app guides](/tags/guide/) to start earning passive income.*
