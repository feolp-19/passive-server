---
title: "Storj: Rent Out Your Extra Hard Drive Space"
date: 2026-01-30
description: "Complete Storj node operator guide. Turn your unused hard drive storage into passive income by joining the decentralized cloud network."
tags: ["apps", "guide", "storj", "storage"]
categories: ["Apps"]
cover:
  image: "/images/storj.png"
  alt: "Storj decentralized storage"
ShowToc: true
TocOpen: true
---

## What is Storj?

Storj is a **decentralized cloud storage network**. Instead of companies like AWS owning all the servers, Storj distributes data across thousands of individual "node operators" - people like you and me!

You get paid to store encrypted pieces of other people's files.

## Why I Love Storj

- ✅ **Truly passive** - Once set up, it just runs
- ✅ **Uses hardware you already have** - Old HDDs work great
- ✅ **Decentralized** - You're part of something bigger
- ✅ **Consistent income** - Gets better over time
- ✅ **Professional payouts** - Monthly via crypto

## My Setup & Earnings

**Hardware:**
- 4TB Seagate external drive
- Connected to my Mac Mini via USB 3.0

**Earnings:**

| Month | Data Stored | Earnings |
|-------|-------------|----------|
| January 2026 | 2.1 TB | $8.40 |
| December 2025 | 1.8 TB | $7.20 |
| November 2025 | 1.5 TB | $6.00 |

**Current rate: ~$4/TB/month** for stored data (plus egress fees)

## Important: Storj Takes Time

⚠️ **Be patient!** Storj nodes take 6-12 months to fill up because:

1. New nodes need to build reputation
2. Data is distributed gradually
3. Held amount period (first 15 months, some earnings held)

But once established, it's **very consistent income**!

## Requirements

Before starting, make sure you have:

- [ ] **Minimum 500GB** dedicated storage (more is better)
- [ ] **Always-on computer** (99.5%+ uptime required!)
- [ ] **Fast, stable internet** (upload speed matters)
- [ ] **Static IP or DDNS** solution
- [ ] **Open port** (28967 by default)
- [ ] **Linux/Windows/Mac** computer

## How to Get Started

### Step 1: Get an Auth Token

👉 **[Sign up for Storj](YOUR_STORJ_REFERRAL_LINK)** 👈

1. Create a Storj account
2. Generate an auth token for your node
3. Save this token - you'll need it!

### Step 2: Prepare Your Storage

Dedicated drive is best:

```bash
# Format drive (if needed)
sudo mkfs.ext4 /dev/sdb1

# Create mount point
sudo mkdir -p /mnt/storj

# Mount the drive
sudo mount /dev/sdb1 /mnt/storj

# Add to /etc/fstab for auto-mount
echo '/dev/sdb1 /mnt/storj ext4 defaults 0 2' | sudo tee -a /etc/fstab
```

### Step 3: Generate Identity

This takes several hours (CPU intensive):

```bash
# Download identity tool
curl -L https://github.com/storj/storj/releases/latest/download/identity_linux_amd64.zip -o identity.zip
unzip identity.zip

# Generate identity (takes 4-24 hours!)
./identity create storagenode
```

### Step 4: Authorize Identity

```bash
./identity authorize storagenode <your-auth-token>
```

### Step 5: Run with Docker

```bash
docker run -d --restart unless-stopped \
  --name storagenode \
  -p 28967:28967/tcp \
  -p 28967:28967/udp \
  -p 14002:14002 \
  -e WALLET="0xYOUR_ETHEREUM_WALLET" \
  -e EMAIL="your@email.com" \
  -e ADDRESS="your.ddns.address:28967" \
  -e STORAGE="4TB" \
  -v /path/to/identity:/app/identity \
  -v /mnt/storj:/app/config \
  storjlabs/storagenode:latest
```

### Step 6: Open Port 28967

On your router:
1. Forward port 28967 (TCP + UDP) to your server
2. Test at https://www.yougetsignal.com/tools/open-ports/

## Dashboard & Monitoring

Access your local dashboard at:
```
http://localhost:14002
```

You'll see:
- Data stored
- Bandwidth used
- Earnings estimate
- Uptime stats

## Tips for Success

1. **Uptime is critical** - Aim for 99.5%+
2. **Use UPS** - Power outages hurt your score
3. **Wired internet** - More reliable than WiFi
4. **Monitor your node** - Check dashboard weekly
5. **Don't restart unnecessarily** - Every restart affects reputation

## Payout Information

- **Minimum**: ~$4 (adjustable)
- **Payment**: STORJ token (ERC-20) or zkSync
- **Schedule**: Monthly, around the 10th

## Earnings Breakdown

You earn for:
- **Storage**: $1.50/TB/month
- **Egress**: $2.00/TB downloaded
- **Repair egress**: $2.00/TB

Real-world earnings are typically $3-5/TB/month.

## Is Running a Storj Node Worth It?

**Pros:**
- Passive income from hardware you own
- Supports decentralized internet
- Gets more profitable over time

**Cons:**
- Slow to fill up (6-12 months)
- Requires 99.5%+ uptime
- First 15 months have held earnings

**My verdict:** If you have spare storage and a 24/7 computer, absolutely worth it!

## FAQ

**Q: Can I use an external USB drive?**
A: Yes! That's exactly what I use.

**Q: How long until I see real money?**
A: 3-6 months for meaningful income.

**Q: What if my node goes offline?**
A: Short outages are fine. Extended downtime = data loss = penalties.

**Q: Can I run multiple nodes?**
A: One node per IP address.

---

## Start Your Storj Node

👉 **[Get Started with Storj](YOUR_STORJ_REFERRAL_LINK)** 👈

---

*Running a Storj node? Share your experience in the comments!*
