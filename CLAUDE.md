# Project Guide

## Workflow
- **Always commit and push changes** unless told otherwise

## Overview
Personal blog built with Astro, using a Greptile-inspired dark-mode-first design.

## Commands
- `npm run dev` - Start dev server at http://localhost:4321
- `npm run build` - Build for production
- `npm run preview` - Preview production build

## Adding Blog Posts

Create a markdown file in `src/content/blog/`:

```markdown
---
title: "Post Title"
description: "Short description for SEO"
pubDate: 2024-01-20
tags: ["tag1", "tag2"]
featured: false
draft: false
---

Content here...
```

### Frontmatter Fields
| Field | Required | Description |
|-------|----------|-------------|
| `title` | Yes | Post title |
| `description` | Yes | Short summary |
| `pubDate` | Yes | Date (YYYY-MM-DD) |
| `tags` | No | Array of strings |
| `featured` | No | Show on homepage |
| `draft` | No | Hide from site |

## Images

**Always use the `public/images/` folder approach.**

1. Place images in `public/images/`
2. Reference in markdown with absolute path:

```markdown
![Alt text](/images/my-image.png)
```

Do NOT use co-located images or complex asset imports.

## Project Structure
```
src/
├── components/     # Astro components
├── layouts/        # Page layouts
├── pages/          # Routes
├── content/blog/   # Markdown posts
└── styles/         # CSS files

public/
├── images/         # Blog images
├── favicon.svg
└── robots.txt
```

## Design System

### Colors (CSS variables in src/styles/variables.css)
- Background: `--color-bg-primary: #0a0a0a`
- Text: `--color-text-primary: #fafafa`
- Accent: `--color-accent-primary: #00ff88` (neon green)

### Typography
- Headlines: Crimson Pro (serif)
- Body: Geist Mono (monospace)

### Key Patterns
- `[BRACKETED]` tag badges
- Horizontal rule dividers between sections
- Dark mode default, light mode via toggle

## Deployment
Push to GitHub → Import on Vercel → Auto-deploys on push
