---
title: "Building a Theme System with CSS Custom Properties"
description: "Learn how to create a flexible dark/light mode theme system using CSS variables without any JavaScript frameworks."
pubDate: 2024-01-10
tags: ["css", "theming", "design-systems"]
featured: true
draft: false
---

CSS Custom Properties (also known as CSS Variables) provide a powerful way to build maintainable theme systems. Unlike preprocessor variables, they're live in the browser and can be changed dynamically.

## Defining Your Color Palette

Start by defining your base colors on the `:root` element:

```css
:root {
  --color-bg-primary: #0a0a0a;
  --color-text-primary: #fafafa;
  --color-accent: #00ff88;
}
```

## Light Mode Overrides

Use a data attribute to switch themes without JavaScript complexity:

```css
[data-theme="light"] {
  --color-bg-primary: #fafafa;
  --color-text-primary: #0a0a0a;
  --color-accent: #00aa55;
}
```

## Fluid Typography

CSS custom properties work beautifully with `clamp()` for fluid typography:

```css
:root {
  --text-base: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
  --text-xl: clamp(1.25rem, 1.1rem + 0.75vw, 1.5rem);
}
```

## Benefits Over Preprocessors

1. **Live updates** - Change values at runtime
2. **Cascade aware** - Respect specificity and inheritance
3. **JavaScript integration** - Read and write from JS
4. **No build step** - Work directly in the browser

## Implementing Theme Toggle

A simple theme toggle can be just a few lines:

```javascript
const toggle = document.getElementById('theme-toggle');
toggle.addEventListener('click', () => {
  const current = document.documentElement.getAttribute('data-theme');
  const next = current === 'dark' ? 'light' : 'dark';
  document.documentElement.setAttribute('data-theme', next);
  localStorage.setItem('theme', next);
});
```

CSS Custom Properties have transformed how we build design systems. They're now my go-to approach for any theming needs.
