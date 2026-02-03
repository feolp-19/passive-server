---
title: "How I Earn $40/Month From an Old Mac Mini"
date: 2026-02-03
description: "A complete guide to setting up a home server for passive income using bandwidth sharing and storage rental apps."
tags: ["guide", "passive-income", "home-server"]
categories: ["Guides"]
keywords: ["passive income home server", "bandwidth sharing", "make money old computer", "Mac Mini server"]
ShowToc: true
TocOpen: true
---

I turned an old Mac Mini into a passive income machine that earns around $40/month with minimal effort. Here's exactly how I set it up.

## The Hardware

- **Mac Mini** (2014 model)
- **Linux Mint XFCE** - lightweight, stable, perfect for 24/7 operation
- **4TB Seagate External Drive** - for Storj storage node

Total investment: Hardware I already had lying around.

## The Apps

I run multiple apps simultaneously to maximize earnings:

| App | Type | Monthly Earnings |
|-----|------|------------------|
| Grass | Bandwidth | ~$15 |
| Honeygain | Bandwidth | ~$10 |
| EarnFM | Bandwidth | ~$5 |
| Traffmonetizer | Bandwidth | ~$4 |
| Storj | Storage | ~$8 |
| **Total** | | **~$42** |

## Setting Up Each App

### Grass

Grass pays the highest of all bandwidth apps. Setup:

1. Create an account at the Grass website
2. Install the browser extension or desktop app
3. Keep it running 24/7

### Honeygain

The most established bandwidth sharing platform:

1. Sign up (referral links often include a $5 bonus)
2. Download the app for your OS
3. Enable Content Delivery for higher earnings
4. Set to auto-start on boot

### EarnFM

Unique because it also pays for music streaming:

1. Create an account
2. Install the bandwidth sharing app
3. Optionally stream music for extra earnings

### Traffmonetizer

Works well alongside other apps:

1. Sign up and get your device token
2. Run via Docker for reliability:

```bash
docker run -d --restart always \
  --name traffmonetizer \
  traffmonetizer/cli:latest \
  start accept --token YOUR_TOKEN
```

### Storj

Rent out your extra storage space:

1. Generate identity (takes several hours)
2. Get an auth token from Storj
3. Run the storage node via Docker
4. Forward port 28967 on your router

Note: Storj takes 6-12 months to fill up, but earnings grow over time.

## Linux Mint Setup Tips

### Auto-login

For a headless server, enable auto-login:

```bash
sudo nano /etc/lightdm/lightdm.conf
```

Add:
```
autologin-user=yourusername
```

### Startup Applications

Add your apps to startup:
- Menu → Preferences → Startup Applications
- Add each app

### Keep System Updated

```bash
sudo apt update && sudo apt upgrade -y
```

### Monitor Resources

```bash
htop
```

## Power Consumption

The Mac Mini uses about 10-15W idle. At $0.15/kWh:
- Monthly power cost: ~$1.50
- Net profit: ~$40

## Realistic Expectations

- **Month 1-2**: Earnings ramp up slowly
- **Month 3+**: Consistent $30-50/month
- **Storj**: Takes 6-12 months to fill, then stable income

This won't make you rich, but it's genuinely passive income from hardware that would otherwise collect dust.

## Conclusion

With a few hours of setup, an old computer can generate $30-50/month indefinitely. The key is running multiple apps and maintaining high uptime.

Questions? Check the other guides on this site for detailed setup instructions for each app.
