---
title: "How I Built This Blog with Astro"
description: "A behind-the-scenes look at building a fast, minimal blog with Astro, CSS custom properties, and a Greptile-inspired design."
pubDate: 2024-01-21
tags: ["astro", "css", "web-development", "design"]
featured: true
draft: false
---

I recently rebuilt my personal blog from scratch. The goal was simple: create something fast, minimal, and easy to maintain. No complex CMS, no heavy JavaScript frameworks—just markdown files and clean code.

Here's how it came together.

## Why Astro?

I chose Astro for one reason: it ships zero JavaScript by default. For a content-focused site like a blog, that's exactly what you want. Every kilobyte matters for performance, and Astro lets you stay lean.

Astro also has excellent support for markdown through Content Collections. You define a schema, drop markdown files in a folder, and Astro handles the rest—type-safe frontmatter, automatic page generation, and built-in syntax highlighting.

```bash
npm create astro@latest my-blog
```

That's all it takes to get started.

## The Design: Greptile-Inspired

I wanted a technical, minimal aesthetic. The kind of design that feels at home for a developer blog. I found inspiration in Greptile's website—dark backgrounds, serif headlines, monospace body text, and neon accent colors.

The key design elements:

- **Dark-mode first** with a light mode toggle
- **Typography pairing**: Crimson Pro (serif) for headlines, Geist Mono for body text
- **Neon green accents** (`#00ff88`) for links and hover states
- **Bracketed tags** like `[JAVASCRIPT]` for a technical feel
- **Generous whitespace** with horizontal rule dividers between sections

The result feels clean but distinctive. It signals "developer" without being sterile.

## CSS Custom Properties for Theming

I avoided CSS frameworks entirely. Instead, I built a theming system with CSS custom properties. This keeps the codebase simple and gives complete control over the design.

The variables live in a single file:

```css
:root {
  --color-bg-primary: #0a0a0a;
  --color-text-primary: #fafafa;
  --color-accent-primary: #00ff88;
  --font-serif: 'Crimson Pro', Georgia, serif;
  --font-mono: 'Geist Mono', monospace;
}
```

Light mode is just an override:

```css
[data-theme="light"] {
  --color-bg-primary: #fafafa;
  --color-text-primary: #0a0a0a;
  --color-accent-primary: #00aa55;
}
```

When the user toggles themes, I flip a `data-theme` attribute on the `<html>` element. The CSS handles the rest. No JavaScript required for the actual styling—just a few lines to toggle the attribute and persist the choice to localStorage.

## Content Collections

Astro's Content Collections are the backbone of the blog. I defined a schema with Zod for type-safe frontmatter:

```typescript
const blogCollection = defineCollection({
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.coerce.date(),
    tags: z.array(z.string()).default([]),
    featured: z.boolean().default(false),
    draft: z.boolean().default(false),
  }),
});
```

Now if I forget a required field or make a typo, Astro catches it at build time. No more broken posts in production.

Querying posts is straightforward:

```astro
const posts = await getCollection('blog', ({ data }) => {
  return data.draft !== true;
});
```

I can filter drafts, sort by date, and grab featured posts—all type-safe.

## The Theme Toggle

Implementing dark/light mode without a flash of wrong content requires a small trick. The theme script runs inline in the `<head>`, before the page renders:

```javascript
const theme = (() => {
  if (localStorage.getItem('theme')) {
    return localStorage.getItem('theme');
  }
  if (window.matchMedia('(prefers-color-scheme: light)').matches) {
    return 'light';
  }
  return 'dark';
})();
document.documentElement.setAttribute('data-theme', theme);
```

This checks localStorage first, falls back to system preference, and defaults to dark. Because it runs before paint, there's no flash.

The toggle button is a simple Astro component with a click handler that flips the attribute and saves to localStorage.

## Project Structure

I kept the structure flat and predictable:

```
src/
├── components/     # Header, Footer, ArticleCard, etc.
├── layouts/        # BaseLayout, BlogPostLayout
├── pages/          # Routes (index, blog/, tags/)
├── content/blog/   # Markdown posts
└── styles/         # CSS files
```

Each component is self-contained with scoped styles. No global CSS collisions, no specificity wars.

## Performance

The production build generates static HTML. No client-side JavaScript except for the theme toggle. The result:

- **Fast initial load** — just HTML and CSS
- **Instant navigation** — no hydration delay
- **SEO-friendly** — everything is server-rendered

I'm targeting 95+ Lighthouse scores across the board. With Astro's defaults and minimal CSS, that's achievable without optimization heroics.

## Deployment

Deployment is simple: push to GitHub, Vercel picks it up automatically. No build configuration needed—Vercel detects Astro and does the right thing.

The RSS feed and sitemap are generated at build time via Astro integrations:

```javascript
import sitemap from '@astrojs/sitemap';

export default defineConfig({
  site: 'https://yourdomain.com',
  integrations: [sitemap()],
});
```

## What I Learned

Building from scratch clarified what I actually need from a blog:

1. **Markdown is enough.** No CMS, no database, no admin panel. Just files.
2. **CSS custom properties are powerful.** Theming without a framework is simpler than I expected.
3. **Astro is excellent for content sites.** The developer experience is smooth, and the output is fast.

The entire site is about 30 files. I understand every line. When something breaks, I know where to look.

That's the real benefit of building it yourself.
