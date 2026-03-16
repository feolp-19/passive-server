---
title: "I Gave an AI Full Control of My Passive Income Server (OpenClaw Setup)"
date: 2026-03-22T09:00:00+01:00
description: "How I set up OpenClaw as a 24/7 AI manager for my home server. It monitors Docker containers, restarts services, and reports via Telegram — all for free."
slug: "openclaw-ai-server-manager"
tags: ["guide", "home-server", "linux", "ai"]
categories: ["Guides", "AI"]
keywords: ["OpenClaw setup guide", "AI server manager", "OpenClaw Telegram", "home server automation", "AI assistant Linux", "OpenClaw free setup", "personal AI agent 2026"]
ShowToc: true
TocOpen: true
faq:
  - question: "What is OpenClaw?"
    answer: "OpenClaw is an open-source AI assistant that runs on your computer 24/7. You control it via WhatsApp, Telegram, or Discord, and it can execute commands, monitor services, and automate tasks."
  - question: "Does OpenClaw cost money?"
    answer: "OpenClaw itself is free and open source. It needs an LLM API to function. You can use Google Gemini or Groq's free tiers for zero monthly cost."
  - question: "Can OpenClaw manage Docker containers?"
    answer: "Yes. OpenClaw has full shell access and can run any command on your server, including docker ps, docker restart, and monitoring scripts."
  - question: "Is it safe to give an AI control of your server?"
    answer: "OpenClaw runs locally on your machine — your data never leaves your hardware. You control what permissions it has, and you can configure it to ask before taking critical actions."
---

I'm leaving my home server unattended for 3 months.

Eight passive income apps, a [Storj node](/storj-node-setup-guide/), and a [tech blog](/) — all running on a single Mac Mini.

My solution? I hired an AI to manage it.

---

## The Problem

My [passive income server](/old-mac-mini-passive-income-machine/) runs 24/7. But things break:

- The [Storj](/storj-node-setup-guide/) external drive unmounts randomly
- Docker containers occasionally crash
- [Grass](/grass-passive-income-guide/) needs a virtual display to run
- Disk space fills up over time

I already have automated fixes (watchdog scripts, Docker restart policies), but I wanted something smarter. Something I could **text from my phone** and get a real answer from my server.

Enter [OpenClaw](https://openclaw.ai/).

---

## What is OpenClaw?

OpenClaw is an open-source AI assistant that lives on your computer. Think of it as a coworker who:

- Never sleeps
- Knows your entire system
- Can run any command
- Reports to you via Telegram, WhatsApp, or Discord

It's not just a chatbot. It **does things**. You text "restart honeygain" and it runs `docker restart honeygain` on your server. You ask "how much disk space is left?" and it runs `df -h` and tells you.

---

## Why I Set It Up

I'm going away for 3 months with no physical access to the server. I already had:

| Automation | Purpose |
|-----------|---------|
| Docker `--restart=always` | Auto-restart crashed containers |
| Watchdog cron job | Remount Storj drive every 2 min |
| Watchtower | Auto-update Docker images |
| Tailscale | Remote SSH from anywhere |

But none of these could **tell me** what's happening. I had to SSH in and check manually. OpenClaw changes that — it monitors proactively and reports via Telegram.

---

## The Setup (Free, 15 Minutes)

### Prerequisites

- A Linux server (mine runs [Linux Mint XFCE](/linux-mint-home-server-setup/))
- Node.js installed
- A free LLM API key (Google Gemini or Groq)
- A Telegram account

### Step 1: Install OpenClaw

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### Step 2: Run Onboarding

```bash
openclaw onboard
```

It asks for your LLM provider. Pick **Google Gemini** and grab a free API key from [aistudio.google.com/apikey](https://aistudio.google.com/apikey).

### Step 3: Connect Telegram

Create a bot via [@BotFather](https://t.me/BotFather) in Telegram:

1. Send `/newbot`
2. Name it whatever you want
3. Copy the bot token

Add it to OpenClaw:

```bash
openclaw channels add --channel telegram --token YOUR_BOT_TOKEN
```

### Step 4: Approve Yourself

Message your bot in Telegram. It will show a pairing code. Approve it:

```bash
openclaw pairing approve telegram YOUR_CODE
```

### Step 5: Done

Message your bot: "Hi, what can you do?"

It responds. You now have an AI managing your server via Telegram.

---

## What I Told It To Do

I gave my OpenClaw a clear job description:

1. **Monitor** all Docker containers every few hours
2. **Fix Storj** if the drive unmounts (remount and restart)
3. **Check Grass** systemd service status
4. **Research** new passive income apps
5. **Report** daily health summaries
6. **Alert** me immediately if anything is down

It remembers these instructions permanently and acts on them proactively.

---

## Real Example

Me in Telegram: *"Health check"*

Bot responds:

> All 8 Docker containers running:
> - storagenode: Up 18 hours
> - honeygain: Up 5 weeks
> - earnfm-client: Up 5 weeks
> - traffmonetizer: Up 2 weeks
> - pawnsapp: Up 5 weeks
> - psclient: Up 5 weeks
> - repocket: Up 5 weeks
> - watchtower: Up 5 weeks (healthy)
>
> Grass service: active
> Disk: 18GB/457GB used (4%)
> Storj drive: mounted, 526GB stored

All from a Telegram message on my phone. No SSH, no terminal.

---

## The Cost: $0

OpenClaw is open source. The LLM API is the only potential cost, but:

| Provider | Cost | Limits |
|----------|------|--------|
| Google Gemini | Free | 15 req/min, 1M tokens/min |
| Groq | Free | 30 req/min, 14,400/day |

For server monitoring — a few commands per day — the free tiers are more than enough.

I run both as fallbacks. If one hits a rate limit, OpenClaw switches to the other.

---

## What It Can't Do

Being honest:

- **It's not perfect** — Free LLMs are less capable than paid ones
- **Rate limits exist** — Heavy usage can temporarily lock you out
- **It can make mistakes** — Don't let it run destructive commands without approval
- **Not a replacement for proper automation** — Watchdog scripts and Docker restart policies are still essential

OpenClaw is the **monitoring layer on top** of existing automation, not a replacement for it.

---

## Should You Set This Up?

**Yes, if:**
- You run a home server and want remote monitoring
- You're comfortable with basic terminal commands
- You want to manage your server from your phone

**Skip it if:**
- Your server doesn't need active monitoring
- You're always near your server
- You don't want to manage API keys

---

## My Full Automation Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Container management | [Docker](/docker-beginners-guide/) | Run all apps in isolation |
| Auto-restart | Docker restart policies | Recover from crashes |
| Auto-updates | Watchtower | Keep images current |
| Drive monitoring | Watchdog cron | Remount Storj drive |
| Remote access | Tailscale | SSH from anywhere |
| **AI manager** | **OpenClaw** | **Monitor, fix, report via Telegram** |

Each layer adds resilience. Together, the server runs itself for months.

---

## Get Started

- [OpenClaw](https://openclaw.ai/) — Download and docs
- [My server setup guide](/linux-mint-home-server-setup/) — Start from scratch
- [All my apps](/apps/) — The full passive income stack

---

*The future is an AI crab managing your passive income while you're on the other side of the world. And it's free.*
