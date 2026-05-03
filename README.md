# JALT Website

Hugo-based website for Just A Little Technical.

## Structure

```
.
├── config.yaml              # Hugo configuration with site metadata, menus, taxonomies
├── content/                 # Content pages and posts
│   ├── _index.md           # Home page
│   ├── about.md            # About page
│   ├── services/           # Services section
│   ├── contact.md          # Contact page
│   ├── blog/               # Blog posts
│   ├── podcast/            # Podcast episodes
│   ├── videos/             # Video content
│   └── resources/          # Resources/downloads
├── layouts/                # HTML templates
│   ├── _default/           # Default layouts
│   │   ├── baseof.html     # Base layout
│   │   ├── single.html     # Single page template (with JSON-LD schema)
│   │   └── list.html       # List/index template
│   ├── index.html          # Home page template
│   ├── podcast/            # Podcast layouts
│   │   ├── list.html       # Podcast index
│   │   └── list.rss.xml    # Podcast RSS feed (iTunes compatible)
│   ├── partials/           # Partial templates
│   │   ├── header.html     # Navigation header
│   │   └── footer.html     # Footer
│   └── shortcodes/         # Reusable shortcodes
│       ├── youtube.html    # YouTube embed
│       ├── podcast-embed.html  # Podcast player
│       └── callout.html    # Callout/info box
├── assets/
│   └── css/
│       └── style.css       # Main stylesheet (JALT palette, responsive)
├── static/                 # Static files (favicon, images, downloads)
├── .github/workflows/      # GitHub Actions
│   ├── deploy-pages.yml    # Deploy to GitHub Pages
│   └── deploy-self-hosted.yml  # Deploy to trixie01 via rsync
├── .gitignore
├── .editorconfig
└── README.md              # This file
```

## Front Matter Fields

### Blog Posts
```yaml
---
title: "Post Title"
date: 2024-05-02
draft: false              # Set to true for draft posts
tags: [tag1, tag2]
categories: [cat1]
cover: /images/blog/2024/05/cover.jpg
summary: "One-line summary for previews"
---
```

### Podcast Episodes
```yaml
---
title: "Episode Title"
date: 2024-05-02
season: 1
episode: 1
duration: "42:30"
audio: "https://podcast.justalittletechnical.com/audio/s1-e1.mp3"
artwork: /images/podcast/artwork.jpg
guest: "Guest Name (optional)"
transcript: "Full episode transcript..."
---
```

### Video Content
```yaml
---
title: "Video Title"
date: 2024-05-02
tags: [tag1]
cover: /images/videos/thumbnail.jpg
summary: "Video description"
---

{{< youtube "VIDEO_ID" >}}
```

## Shortcodes

### YouTube
```markdown
{{< youtube "dQw4w9WgXcQ" >}}
```

### Podcast Embed
```markdown
{{< podcast-embed url="https://podcast.justalittletechnical.com/audio/episode.mp3" title="Listen" >}}
```

### Callout
```markdown
{{< callout type="info" >}}
This is an info box.
{{< /callout >}}

{{< callout type="warning" >}}
This is a warning.
{{< /callout >}}

{{< callout type="success" >}}
This is a success message.
{{< /callout >}}
```

## Build & Development

### Local Development
```bash
hugo server --gc --draft
```

Opens site at `http://localhost:1313/`

### Production Build
```bash
hugo --gc --minify
```

Output in `/public/`

## Deployment

### GitHub Pages
Push to `main` branch. GitHub Actions automatically builds and deploys to `github.com/mike0615/jalt-website`.

Enable in repo settings:
1. Go to Settings → Pages
2. Set source to "GitHub Actions"

### Self-Hosted (trixie01)
Push to `main` branch. GitHub Actions builds and rsyncs `/public/` to trixie01.

Requires secrets:
- `DEPLOY_KEY`: SSH private key (Ed25519)
- `DEPLOY_HOST`: trixie01 hostname/IP
- `DEPLOY_USER`: Deploy user on trixie01

Caddy serves from `/opt/jalt-website/public/`

## Configuration

All site settings in `config.yaml`:

- `baseURL`: Site URL
- `title`: Site title
- `params.primaryColor`, etc: JALT brand colors
- `params.plausibleDomain`: Analytics domain
- `menu.main`: Navigation menu
- `taxonomies`: Tags, categories, series

## Content Strategy

1. **Home**: Featured content, newsletter signup, CTA
2. **Blog**: Long-form technical articles (2-4/month)
3. **Podcast**: Episodes with transcripts (1/week)
4. **Videos**: YouTube embeds and transcripts (1-2/month)
5. **Resources**: Free downloads, templates, guides
6. **Services**: Service offerings and case studies
7. **About**: Mike's story and JALT mission
8. **Contact**: Lead capture form (posts to Trixie API)

## SEO & Accessibility

- **SEO**: Sitemaps, robots.txt, schema.org JSON-LD, OpenGraph tags
- **Accessibility**: WCAG 2.2 AA (color contrast, keyboard nav, semantic HTML)
- **Performance**: LCP < 2.0s, CLS < 0.1, JS < 50KB gzipped
- **Analytics**: Plausible (privacy-friendly, cookie-free)

## License

© 2024 Just A Little Technical. All rights reserved.
