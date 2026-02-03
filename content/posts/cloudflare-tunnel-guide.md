---
title: "Exposing Home Services with Cloudflare Tunnel"
date: 2026-02-01
description: "How to securely expose your home server to the internet using Cloudflare Tunnel without opening ports."
tags: ["tutorial", "cloudflare", "home-server", "self-hosting"]
categories: ["Tutorials"]
keywords: ["Cloudflare Tunnel", "expose home server", "self-hosting", "cloudflared", "no port forwarding"]
ShowToc: true
TocOpen: true
---

Cloudflare Tunnel lets you expose services running on your home server without opening ports on your router. It's free and more secure than traditional port forwarding.

## How It Works

```
Internet → Cloudflare → Tunnel → Your Server
```

Your server creates an outbound connection to Cloudflare. No inbound ports needed.

## Prerequisites

- A domain on Cloudflare (free plan works)
- A server running Linux
- A service you want to expose

## Installing Cloudflared

### Debian/Ubuntu

```bash
curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null

echo 'deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared focal main' | sudo tee /etc/apt/sources.list.d/cloudflared.list

sudo apt update && sudo apt install cloudflared
```

### Verify Installation

```bash
cloudflared --version
```

## Authentication

Log in to your Cloudflare account:

```bash
cloudflared tunnel login
```

This opens a browser. Select your domain and authorize.

## Creating a Tunnel

```bash
cloudflared tunnel create my-tunnel
```

This creates a tunnel and outputs a tunnel ID. Save it.

## Configuration

Create a config file:

```bash
mkdir -p ~/.cloudflared
nano ~/.cloudflared/config.yml
```

Example configuration:

```yaml
tunnel: YOUR_TUNNEL_ID
credentials-file: /home/user/.cloudflared/YOUR_TUNNEL_ID.json

ingress:
  # Route subdomain to local service
  - hostname: app.yourdomain.com
    service: http://localhost:3000
  
  # Another service
  - hostname: api.yourdomain.com
    service: http://localhost:8080
  
  # Catch-all (required)
  - service: http_status:404
```

## DNS Setup

Route your subdomain to the tunnel:

```bash
cloudflared tunnel route dns my-tunnel app.yourdomain.com
```

Or manually add a CNAME in Cloudflare DNS:
- Type: CNAME
- Name: app
- Target: YOUR_TUNNEL_ID.cfargotunnel.com

## Running the Tunnel

### Test It

```bash
cloudflared tunnel run my-tunnel
```

### Install as a Service

```bash
sudo cloudflared service install
sudo systemctl enable cloudflared
sudo systemctl start cloudflared
```

### Check Status

```bash
sudo systemctl status cloudflared
```

## Multiple Services

One tunnel can serve multiple hostnames. Just add more entries to the ingress:

```yaml
ingress:
  - hostname: blog.example.com
    service: http://localhost:2368
  - hostname: files.example.com
    service: http://localhost:8080
  - hostname: code.example.com
    service: http://localhost:8443
  - service: http_status:404
```

## Security Considerations

### Access Control

Add Cloudflare Access for authentication:

1. Go to Cloudflare Zero Trust dashboard
2. Create an Access application
3. Configure authentication (email, SSO, etc.)

### Limit to Cloudflare IPs

Since traffic comes through Cloudflare, your origin sees Cloudflare IPs. This is fine, but be aware of it for logging.

## Troubleshooting

### Check Tunnel Status

```bash
cloudflared tunnel info my-tunnel
```

### View Logs

```bash
journalctl -u cloudflared -f
```

### Common Issues

**502 Bad Gateway**: Your local service isn't running or is on the wrong port.

**DNS not resolving**: Wait a few minutes for DNS propagation, or check your CNAME record.

**Tunnel not starting**: Check credentials file path in config.yml.

## Conclusion

Cloudflare Tunnel is the easiest way to expose home services:

- No port forwarding required
- Free with any Cloudflare plan
- Built-in DDoS protection
- Optional access control

Once set up, it just works. Your services are available from anywhere without exposing your home IP.
