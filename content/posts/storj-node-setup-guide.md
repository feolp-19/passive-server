---
title: "Found Money: How I Turned an Old 4TB Drive into a Crypto Node (Storj)"
date: 2026-03-01T09:00:00+01:00
description: "Complete Storj node setup guide for Linux. Learn how to monetize an unused hard drive by running a decentralized storage node and earning STORJ tokens."
slug: "storj-node-setup-guide"
tags: ["guide", "passive-income", "docker", "storj", "crypto"]
categories: ["Crypto", "Storage", "Passive Income"]
keywords: ["Storj node setup", "monetize unused hard drive", "Storj docker linux", "passive income hdd mining", "decentralized storage", "STORJ tokens", "Linux Mint server", "Storj vetting"]
ShowToc: true
TocOpen: true
faq:
  - question: "How long does Storj vetting take?"
    answer: "The vetting process takes 1-2 months. During this time, the network gradually increases trust in your node and sends more data."
  - question: "How much can I earn with Storj?"
    answer: "Earnings depend on your storage size and uptime. A 4TB node can earn $3-10 per month once fully vetted. It's a long-term passive income stream."
  - question: "What uptime does Storj require?"
    answer: "Storj requires 99.5% uptime. If your node goes offline frequently, you'll lose reputation and data."
  - question: "Can I use an external USB drive for Storj?"
    answer: "Yes, but use a powered USB hub for stability. Format it as ext4 for best performance on Linux."
---

I was cleaning my desk when I found it.

An old **4TB Seagate Backup Plus** external drive. Sitting in a drawer. Collecting dust. Completely empty.

I almost threw it away.

Then I asked Gemini: *"Can I make money with an empty hard drive?"*

The answer changed everything.

---

## The Discovery: Storj

Gemini introduced me to **Storj** — a decentralized cloud storage network.

Think of it like Airbnb, but for hard drive space:

- People need cloud storage
- You have empty disk space
- Storj connects you and handles the payments

You rent out your unused gigabytes. You get paid in **STORJ tokens** (cryptocurrency).

### Why This Beats GPU Mining

Traditional crypto mining requires:

| Resource | GPU Mining | Storj |
|----------|-----------|-------|
| Hardware | $500+ GPU | Old hard drive |
| Electricity | 200-400 watts | 5-10 watts |
| Noise | Loud fans | Silent |
| Heat | Room heater | Cool |

My 4TB drive uses about **5 watts**. That's less than a phone charger.

I'm not burning electricity to solve math problems. I'm just letting data sit on a spinning disk.

---

## What is Storj?

Storj is a decentralized alternative to Amazon S3, Google Cloud, and Dropbox.

### How It Works

1. **Users upload files** to Storj (encrypted and split into pieces)
2. **Your node stores pieces** of those files
3. **You get paid** for storage and bandwidth

The files are encrypted before they reach you. You can't see what's stored. You just provide the space.

### Why Companies Use Storj

- **80% cheaper** than traditional cloud storage
- **Decentralized** — no single point of failure
- **Fast** — pieces download from multiple nodes simultaneously

Big companies use Storj. Real demand means real payments.

---

## The Setup

This is the technical part. It took me an afternoon, but most of that was waiting for identity generation.

### Prerequisites

- Linux server (I use Linux Mint on my Mac Mini)
- Docker installed
- External drive (or internal with spare space)
- Stable internet with a public IP or port forwarding
- Patience (the identity step takes hours)

---

### Step 1: Generate Your Identity

Storj requires a cryptographic identity to verify your node. This takes **hours** to generate — it's computationally intensive by design.

**Start the identity generation:**

```bash
curl -L https://github.com/storj/storj/releases/latest/download/identity_linux_amd64.zip -o identity_linux_amd64.zip
unzip identity_linux_amd64.zip
chmod +x identity
./identity create storagenode
```

This will run for **2-24 hours** depending on your CPU. Let it run overnight.

**Verify the identity:**

```bash
grep -c BEGIN ~/.local/share/storj/identity/storagenode/ca.cert
grep -c BEGIN ~/.local/share/storj/identity/storagenode/identity.cert
```

You need `ca.cert` to return `2` and `identity.cert` to return `3`.

**Authorize the identity:**

Get an auth token from the [Storj Node Dashboard](https://registration.storj.io/), then:

```bash
./identity authorize storagenode YOUR_AUTH_TOKEN_HERE
```

---

### Step 2: Format the Drive

For best performance on Linux, format your drive as **ext4**.

**Find your drive:**

```bash
lsblk
```

Look for your external drive (e.g., `/dev/sdb`).

**Format it:**

```bash
sudo mkfs.ext4 /dev/sdb1
```

**Create a mount point:**

```bash
sudo mkdir -p /mnt/storage
```

---

### Step 3: Auto-Mount on Reboot

You don't want to manually mount the drive every time the server restarts.

**Get the drive UUID:**

```bash
sudo blkid /dev/sdb1
```

**Edit fstab:**

```bash
sudo nano /etc/fstab
```

Add this line (replace UUID with yours):

```
UUID=your-drive-uuid-here /mnt/storage ext4 defaults 0 2
```

**Test it:**

```bash
sudo mount -a
```

If no errors, you're good. The drive will mount automatically on every boot.

---

### Step 4: Run the Storj Node

Now the main event — the Docker command.

```bash
docker run -d --restart unless-stopped \
  --name storagenode \
  -p 28967:28967/tcp \
  -p 28967:28967/udp \
  -p 14002:14002 \
  -e WALLET="YOUR_WALLET_ADDRESS_HERE" \
  -e EMAIL="YOUR_EMAIL_HERE" \
  -e ADDRESS="YOUR_DOMAIN_OR_IP:28967" \
  -e STORAGE="3.5TB" \
  -v ~/.local/share/storj/identity/storagenode:/app/identity \
  -v /mnt/storage/storj:/app/config \
  storjlabs/storagenode:latest
```

**Replace:**
- `YOUR_WALLET_ADDRESS_HERE` — Your Ethereum wallet for STORJ token payments
- `YOUR_EMAIL_HERE` — Contact email for Storj notifications
- `YOUR_DOMAIN_OR_IP:28967` — Your public address (use DDNS if you have a dynamic IP)
- `3.5TB` — Slightly less than your total capacity (leave room for overhead)

**Port forwarding:**

Make sure port **28967** (TCP and UDP) is forwarded on your router to your server's local IP.

---

### Step 5: Verify It's Running

**Check the container:**

```bash
docker ps | grep storagenode
```

**Access the dashboard:**

Open `http://YOUR_SERVER_IP:14002` in a browser.

You'll see your node status, bandwidth usage, and storage allocation.

---

## The Vetting Process (Reality Check)

Here's the honest part: **you won't get rich on day one**.

Storj uses a trust system called "vetting":

| Phase | Duration | What Happens |
|-------|----------|--------------|
| New Node | Month 1-2 | Network sends small test data |
| Vetting | Month 2-3 | Gradually increases if uptime is good |
| Vetted | Month 3+ | Full traffic, real earnings |

### Why Vetting Exists

Storj can't trust random nodes immediately. What if you:

- Go offline and lose people's data?
- Have an unstable connection?
- Disappear after a week?

The vetting period proves you're reliable. Patience is required.

### My Strategy: Set and Forget

I configured everything, verified it was running, and walked away.

- Check the dashboard weekly
- Let Watchtower auto-update the container
- Collect STORJ tokens monthly

This isn't a get-rich-quick scheme. It's a long-term passive income stream.

---

## Current Status

My node has been running for about a month now. Here's where it stands:

| Metric | Value |
|--------|-------|
| Allocated Space | 3.3 TB |
| Used Space | ~250 GB (growing) |
| Status | Vetting |
| Uptime | 99.8% |

The used space grows slowly at first. After full vetting, I expect to use 1-2 TB within a year.

---

## Power Usage

This is what makes Storj attractive:

| Component | Watts |
|-----------|-------|
| External HDD | 5W |
| Mac Mini (already on) | 0W extra |
| **Total extra cost** | ~$5/year |

Compare that to GPU mining at 200W+ running 24/7. The ROI math is completely different.

---

## Earnings Expectations

Being realistic:

| Storage | Monthly Earnings (Vetted) |
|---------|---------------------------|
| 1 TB | $1-3 |
| 4 TB | $3-10 |
| 8 TB | $8-20 |

It's not life-changing money. But it's money from hardware that was literally sitting in a drawer.

The STORJ token also has upside potential. If the token appreciates, your earnings are worth more retroactively.

---

## Conclusion

I turned a dusty paperweight into a working crypto node.

**The investment:**
- Old hard drive (free — I already had it)
- One afternoon of setup
- Patience for vetting

**The return:**
- Passive income in STORJ tokens
- Learning about decentralized storage
- A use for forgotten hardware

---

## Your Turn

Look around. Check your drawers, closets, and old computer bags.

Got an old hard drive? It could be earning you money right now.

**Resources:**
- [Storj Node Documentation](https://docs.storj.io/node)
- [Storj Node Registration](https://registration.storj.io/)

---

## The Stack So Far

| # | App | Type | Status |
|---|-----|------|--------|
| 1 | Grass | Bandwidth | ✅ Running |
| 2 | Honeygain | Bandwidth | ✅ Running |
| 3 | EarnFM | Bandwidth | ✅ Running |
| 4 | **Storj** | Storage | ✅ Running |

Four passive income streams. Same server. Same electricity bill.

Check out all the [apps I'm running](/apps/) for the complete list.

---

*Found an old drive and set up a node? Let me know how it goes.*
