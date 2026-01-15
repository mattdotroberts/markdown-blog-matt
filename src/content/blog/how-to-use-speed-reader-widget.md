---
title: "How to Add the Speed Reader Widget to Your Website"
description: "Add speed reading capabilities to any website with a single script tag. Learn about display modes, configuration options, and keyboard shortcuts."
pubDate: 2026-01-15
tags: ["javascript", "tutorial", "productivity"]
featured: true
draft: false
---

The Speed Reader widget lets your visitors speed read your content using RSVP (Rapid Serial Visual Presentation) technology. It displays one word at a time with a highlighted anchor letter, helping readers process text faster.

## Quick Start

Add this single line to your website:

```html
<script src="https://speedreader-delta.vercel.app/dist/speed-reader-widget.js"></script>
```

This adds a floating "Speed Read" button in the bottom-right corner. When clicked, it opens a modal that auto-extracts your page content for speed reading.

## Display Modes

### Floating Button (Default)

A fixed-position button that floats in the corner of the screen:

```html
<script src="https://speedreader-delta.vercel.app/dist/speed-reader-widget.js"></script>
```

### Inline Button

Place the button anywhere in your content. It still opens a modal when clicked:

```html
<div id="speed-reader-btn"></div>
<script
  src="https://speedreader-delta.vercel.app/dist/speed-reader-widget.js"
  data-position="inline"
  data-container="#speed-reader-btn">
</script>
```

### Inline Reader

Embed the full reader directly in your page (no button or modal):

```html
<div id="speed-reader"></div>
<script
  src="https://speedreader-delta.vercel.app/dist/speed-reader-widget.js"
  data-mode="inline"
  data-container="#speed-reader">
</script>
```

## Configuration Options

Customize the widget using data attributes:

| Attribute | Options | Default | Description |
|-----------|---------|---------|-------------|
| `data-mode` | `button`, `inline` | `button` | Button shows a trigger that opens a modal. Inline embeds the reader directly. |
| `data-position` | `bottom-right`, `bottom-left`, `top-right`, `top-left`, `inline` | `bottom-right` | Where the button appears. Use `inline` to place it within your content. |
| `data-container` | CSS selector | — | Where to render the widget (required for inline position or mode). |
| `data-theme` | `light`, `dark` | `light` | Color theme. |
| `data-wpm` | `100` - `900` | `300` | Starting words per minute. |
| `data-content` | `auto`, CSS selector | `auto` | What content to read. `auto` extracts the main article. Use a selector like `#article-body` to target specific content. |
| `data-keyboard` | `true`, `false` | `true` | Enable keyboard shortcuts. |
| `data-manual` | — | — | Disable auto-initialization for programmatic control. |

## Examples

### Dark Theme, Top-Left Position, Faster Speed

```html
<script
  src="https://speedreader-delta.vercel.app/dist/speed-reader-widget.js"
  data-theme="dark"
  data-position="top-left"
  data-wpm="450">
</script>
```

### Read Specific Content

Only read content from a specific element:

```html
<script
  src="https://speedreader-delta.vercel.app/dist/speed-reader-widget.js"
  data-content="#article-content">
</script>
```

### Programmatic Control

For advanced use, disable auto-init and control the widget with JavaScript:

```html
<script
  src="https://speedreader-delta.vercel.app/dist/speed-reader-widget.js"
  data-manual>
</script>
<script>
  const widget = new SpeedReaderWidget({
    mode: 'button',
    theme: 'dark',
    wpm: 400,
    onComplete: () => console.log('Finished reading!')
  });

  // Open the reader programmatically
  widget.open();

  // Change speed
  widget.setWPM(500);

  // Load custom text
  widget.loadText('Your custom text here', 'Custom Title');
</script>
```

## Keyboard Shortcuts

When the reader is open:

| Key | Action |
|-----|--------|
| `Space` or `K` | Play / Pause |
| `←` or `J` | Rewind 10 words |
| `→` or `L` | Forward 10 words |
| `Escape` | Close modal |

## How It Works

The widget uses RSVP technology with an Optimal Recognition Point (ORP). Each word is displayed with a red anchor letter slightly left of center—this is where your eye naturally focuses. The word shifts around this fixed point, eliminating eye movement and allowing faster reading.

Readers can adjust speed on the fly without losing their place, and the progress bar shows how far through the article they've read.
