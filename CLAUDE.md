# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Workflow
- **Always commit and push changes** unless told otherwise

## Overview
Personal blog built with Astro 5, using a Greptile-inspired dark-mode-first design. Uses Astro Content Collections for blog posts and projects.

## Commands
- `npm run dev` - Start dev server at http://localhost:4321
- `npm run build` - Build for production
- `npm run preview` - Preview production build

## Architecture

### Content Collections
Content lives in `src/content/` with two collections:
- `blog/` - Blog posts (markdown files)
- `projects/` - Project showcases (markdown files)

Dynamic routes (`[...slug].astro`) use `getCollection()` and `render()` from `astro:content` to generate pages.

### Layouts
- `BaseLayout.astro` - Root layout with meta tags, theme script, header/footer
- `BlogPostLayout.astro` - Wraps BaseLayout for blog posts

### Routing
- `/` - Homepage with featured posts and about section
- `/blog/` - Blog listing
- `/blog/[slug]` - Individual posts
- `/projects/` - Projects grid
- `/projects/[slug]` - Individual projects
- `/tags/` - Tag listing
- `/tags/[tag]` - Posts by tag

## Adding Content

### Blog Posts
Create `src/content/blog/your-post.md`:
```markdown
---
title: "Post Title"
description: "Short description"
pubDate: 2024-01-20
tags: ["tag1", "tag2"]
featured: false
draft: false
---
```

### Projects
Create `src/content/projects/your-project.md`:
```markdown
---
title: "Project Name"
description: "Short description"
date: 2024-01-20
url: "https://example.com"
repo: "https://github.com/user/repo"
tags: ["tag1"]
featured: true
draft: false
---
```

## Images
Place images in `public/images/` and reference with absolute paths:
```markdown
![Alt text](/images/my-image.png)
```

## Design System

### Colors (defined in src/styles/variables.css)
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
