# Passive Income Lab - Hugo Blog

> Mac Mini passive server blog - earn money from your unused bandwidth and storage!

A blog about earning passive income with bandwidth sharing and storage rental apps.

## 🚀 Quick Start

### Prerequisites

- [Hugo](https://gohugo.io/installation/) (extended version)
- Git

### Local Development

```bash
# Clone with submodules (for the theme)
git clone --recursive https://github.com/feolp-19/passive-server.git
cd passive-server

# If you forgot --recursive, add the theme:
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod

# Run local server
hugo server -D

# Open http://localhost:1313
```

### Adding Your Referral Links

Replace `YOUR_*_REFERRAL_LINK` in these files with your actual referral links:

- `content/posts/grass.md`
- `content/posts/honeygain.md`
- `content/posts/earnfm.md`
- `content/posts/traffmonetizer.md`
- `content/posts/storj.md`

### Adding New Posts

```bash
hugo new posts/my-new-post.md
```

Then edit `content/posts/my-new-post.md`.

## 📁 Structure

```
passive-server/
├── content/
│   ├── posts/           # Blog posts
│   │   ├── getting-started.md
│   │   ├── grass.md
│   │   ├── honeygain.md
│   │   ├── earnfm.md
│   │   ├── traffmonetizer.md
│   │   └── storj.md
│   └── about.md         # About page
├── static/
│   └── images/          # Add your images here
├── themes/
│   └── PaperMod/        # Theme (git submodule)
├── hugo.toml            # Site configuration
└── .github/
    └── workflows/
        └── hugo.yml     # GitHub Pages deployment
```

## 🌐 Deployment (GitHub Pages)

1. Create a GitHub repo
2. Push this code to the repo
3. Go to **Settings > Pages**
4. Set Source to **GitHub Actions**
5. The site will auto-deploy on every push!

### Custom Domain (with Cloudflare)

1. In GitHub repo settings, add your custom domain
2. In Cloudflare, add a CNAME record pointing to `feolp-19.github.io`
3. Update `baseURL` in `hugo.toml`

## 📝 Writing Posts

Posts use Markdown with YAML front matter:

```markdown
---
title: "My Post Title"
date: 2026-02-03
description: "SEO description for search engines"
tags: ["apps", "guide"]
categories: ["Guides"]
cover:
  image: "/images/my-image.png"
  alt: "Alt text for SEO"
ShowToc: true
---

Your content here...
```

## 🎨 Customization

Edit `hugo.toml` to:

- Change site title and description
- Update social links
- Modify menu items
- Adjust theme settings

## 📊 SEO Features

This template is optimized for SEO:

- ✅ Semantic HTML
- ✅ Open Graph tags
- ✅ Twitter cards
- ✅ Sitemap.xml
- ✅ RSS feed
- ✅ robots.txt
- ✅ Fast loading (static site)
- ✅ Mobile responsive

## 📄 License

MIT - Do whatever you want with it!
