# Gallery Lightbox — Design Spec

**Date:** 2026-03-31  
**Status:** Approved

## Overview

Replace the placeholder Unsplash images in `src/pages/galeria.astro` with the 7 real workshop photos from `public/galeria/`, and add a vanilla JS lightbox for full-screen viewing.

## Scope

Single file change: `src/pages/galeria.astro`.

## Image Data

7 real photos in `/galeria/`:
- FRS_9654.jpg
- FRS_9697.jpg
- FRS_9708.jpg
- FRS_9712.jpg
- FRS_9714.jpg
- FRS_9726.jpg
- FRS_9801.jpg

Alt texts: descriptive Spanish captions (e.g. "Trabajos en el taller Electromecánica Eprom").

## Gallery Grid

Keep existing layout: `repeat(auto-fill, minmax(320px, 1fr))` grid, 4:3 aspect ratio per card, hover zoom on image. Remove the "fotos de referencia" notice. Each card gets `data-src` and `data-index` attributes for the lightbox.

## Lightbox (vanilla JS)

**Trigger:** click on any gallery card.

**Overlay structure (injected into DOM once):**
```
#lb-overlay  (fixed, full viewport, z-index:9999, bg rgba(0,0,0,0.92))
  #lb-img    (centered <img>, max 90vw / 90vh, object-fit:contain)
  #lb-close  (× button, top-right)
  #lb-prev   (‹ arrow, left-center)
  #lb-next   (› arrow, right-center)
```

**Behavior:**
- Click card → set `currentIndex`, show overlay, load image
- Prev/Next buttons → cycle through images array (wraps around)
- Close: × button, click outside image, or Escape key
- Prevent scroll while open (`document.body.style.overflow = 'hidden'`)

**Styling:** minimal inline styles on overlay elements, consistent with site design (`#F5A623` accent on arrows/close button, `Bebas Neue` font for nav).

## What Does NOT Change

- Page header, layout, animations
- SEO meta tags
- Any other page
