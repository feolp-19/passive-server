---
title: "Docker for Beginners: Run Anything on Your Home Server (No Experience Needed)"
date: 2026-05-17T09:00:00+01:00
description: "A beginner-friendly Docker tutorial. Learn what containers are, how to use them, and why Docker is the best way to run apps on your home server."
slug: "docker-beginners-guide"
tags: ["guide", "docker", "linux", "home-server"]
categories: ["Guides", "Docker"]
keywords: ["Docker for beginners", "Docker tutorial 2026", "what is Docker", "Docker home server", "Docker commands explained", "learn Docker Linux", "container basics"]
ShowToc: true
TocOpen: true
faq:
  - question: "What is Docker in simple terms?"
    answer: "Docker is a tool that runs applications in isolated packages called containers. Each container has everything the app needs, so it works on any computer without conflicts."
  - question: "Is Docker hard to learn?"
    answer: "No. You only need to know a handful of commands to be productive. This guide covers everything you need for a home server."
  - question: "Why use Docker instead of installing apps normally?"
    answer: "Docker keeps apps isolated from each other and your system. No dependency conflicts, easy updates, easy removal, and apps run the same way everywhere."
  - question: "Does Docker slow down my computer?"
    answer: "No. Docker containers share the host's kernel and add virtually no overhead. They're much lighter than virtual machines."
---

Every passive income app on my server runs in Docker. Every single one.

There's a reason for that. Docker makes everything easier — installing, updating, removing, and troubleshooting.

If you've never used Docker, this guide is for you.

---

## What is Docker?

Imagine you want to run an app on your server. Normally you'd:

1. Install dependencies
2. Configure settings
3. Hope it doesn't conflict with other apps
4. Struggle to update it later
5. Leave behind a mess when you remove it

Docker skips all of that.

### Containers in Plain English

A **container** is a self-contained package with everything an app needs to run — the code, libraries, settings, everything.

Think of it like a shipping container:
- Everything inside is self-contained
- It works the same no matter where you put it
- Containers don't interfere with each other
- Easy to add, easy to remove

---

## Installing Docker

On Linux Mint / Ubuntu:

```bash
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
```

Add yourself to the Docker group (so you don't need `sudo` every time):

```bash
sudo usermod -aG docker $USER
```

**Log out and back in** for this to take effect.

Verify it works:

```bash
docker --version
docker run hello-world
```

---

## The 10 Commands You Need

You only need these to manage a home server:

### 1. Run a Container

```bash
docker run -d --restart=always --name myapp image:latest
```

| Flag | Meaning |
|------|---------|
| `-d` | Run in background (detached) |
| `--restart=always` | Auto-restart on crash or reboot |
| `--name myapp` | Give it a memorable name |
| `image:latest` | The app image to run |

### 2. See Running Containers

```bash
docker ps
```

### 3. See All Containers (Including Stopped)

```bash
docker ps -a
```

### 4. View Logs

```bash
docker logs myapp
docker logs myapp --tail 20     # Last 20 lines
docker logs myapp -f            # Follow live
```

### 5. Stop a Container

```bash
docker stop myapp
```

### 6. Start a Stopped Container

```bash
docker start myapp
```

### 7. Restart a Container

```bash
docker restart myapp
```

### 8. Remove a Container

```bash
docker stop myapp
docker rm myapp
```

### 9. Check Resource Usage

```bash
docker stats --no-stream
```

### 10. Clean Up Unused Data

```bash
docker system prune -f
```

That's it. These 10 commands cover 99% of home server management.

---

## Environment Variables

Many apps need configuration. Docker uses `-e` flags for this:

```bash
docker run -d --name myapp \
  -e EMAIL=me@example.com \
  -e PASSWORD=secret123 \
  -e API_KEY=abc123 \
  image:latest
```

Each `-e` sets a variable inside the container. The app reads these instead of config files.

---

## Ports

Some apps need network ports exposed:

```bash
docker run -d --name myapp \
  -p 8080:80 \
  image:latest
```

This maps port 8080 on your server to port 80 inside the container. Access the app at `http://your-server-ip:8080`.

---

## Volumes (Persistent Data)

Containers are temporary by default — data is lost when they're removed. Volumes keep data safe:

```bash
docker run -d --name myapp \
  -v /path/on/server:/path/in/container \
  image:latest
```

For example, [Storj](/storj-node-setup-guide/) stores data on an external drive:

```bash
-v /mnt/storage/storj:/app/config
```

The data lives on your drive, not inside the container.

---

## Real Examples from My Server

Here's how Docker runs my entire passive income stack:

### Honeygain

```bash
docker run -d --restart=always \
  --name honeygain \
  honeygain/honeygain \
  -tou-accept \
  -email YOUR_EMAIL \
  -pass YOUR_PASSWORD \
  -device mac-mini
```

### EarnFM

```bash
docker run -d --restart=always \
  --name earnfm-client \
  earnfm/earnfm-client:latest \
  --token YOUR_TOKEN
```

### Traffmonetizer

```bash
docker run -d --restart=always \
  --name traffmonetizer \
  traffmonetizer/cli_v2 \
  start accept --token YOUR_TOKEN
```

### PacketStream

```bash
docker run -d --restart=always \
  --name psclient \
  -e CID=YOUR_CID \
  packetstream/psclient:latest
```

Each one is a single command. No installation wizards, no dependency hell.

---

## Auto-Updating with Watchtower

Watchtower automatically updates your containers when new versions are released:

```bash
docker run -d --restart=unless-stopped \
  --name watchtower \
  -v /var/run/docker.sock:/var/run/docker.sock \
  containrrr/watchtower \
  --cleanup --interval 86400
```

It checks every 24 hours and updates silently. You never have to manually update.

---

## Troubleshooting

### Container keeps restarting

Check the logs:

```bash
docker logs myapp --tail 50
```

### "Permission denied"

Run with `sudo` or make sure you're in the Docker group:

```bash
groups | grep docker
```

### Container using too much memory

Check usage and restart it:

```bash
docker stats --no-stream
docker restart myapp
```

### Want to start fresh

Remove and recreate:

```bash
docker stop myapp && docker rm myapp
docker run -d ... # Your original command
```

---

## Docker vs. Installing Directly

| Aspect | Direct Install | Docker |
|--------|---------------|--------|
| Installation | Complex | One command |
| Conflicts | Common | Impossible |
| Updates | Manual | Automatic (Watchtower) |
| Removal | Leaves files behind | Clean removal |
| Portability | System-specific | Works anywhere |
| Learning curve | Varies per app | Same commands for everything |

---

## What's Next?

Now that you understand Docker, you can run anything:

- **[Passive income apps](/apps/)** — My complete stack
- **Media servers** — Plex, Jellyfin
- **Home automation** — Home Assistant
- **Network tools** — Pi-hole, AdGuard
- **Anything on Docker Hub** — Thousands of pre-built images

Start with the [home server setup guide](/linux-mint-home-server-setup/) if you haven't set up your server yet.

---

*Docker makes home servers accessible to everyone. If you can copy-paste a command, you can run a server.*
