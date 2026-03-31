# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static website for **Electromecánica Eprom**, an auto repair shop in Palma de Mallorca. Built with Astro 6 + Tailwind CSS v4. All content is in Spanish. The site targets local SEO for automotive services in Mallorca.

## Commands

```bash
npm run dev        # Dev server at localhost:4321
npm run build      # Production build to ./dist/
npm run preview    # Preview production build locally
```

No test framework is configured. No linter is configured.

## Deployment

- **Live URL**: `https://www.electromecanicaeprom.com`
- **Hosting**: GitHub Pages (`onejensen/Electromecanica-Eprom` repo, `gh-pages` branch)
- **CI/CD**: GitHub Actions workflow at `.github/workflows/deploy.yml` — every push to `main` builds and deploys automatically
- **No base path** — the site is served at the domain root. Do NOT add a `base` to `astro.config.mjs`
- **CNAME**: `public/CNAME` contains `www.electromecanicaeprom.com` — do not delete it

## Architecture

- **Astro 6** static site generator with file-based routing (`src/pages/`)
- **Tailwind CSS v4** via Vite plugin (not PostCSS) — configured in `astro.config.mjs`
- **Sitemap** auto-generated via `@astrojs/sitemap`
- `site` in `astro.config.mjs` is `https://www.electromecanicaeprom.com`

### Internal Links & Assets

All internal `href` links use `import.meta.env.BASE_URL` prefix (currently `/`). Follow the same pattern when adding new links or asset references:

```astro
<a href={`${import.meta.env.BASE_URL}contacto/`}>Contacto</a>
<img src={`${import.meta.env.BASE_URL}LOGO_EPROM_.png`} />
```

### Layout & Components

All pages use `src/layouts/Layout.astro` which wraps content with Navbar, Footer, and WhatsAppButton. Every page must pass `title` and `description` props to Layout.

`src/components/SEO.astro` handles meta tags, Open Graph, Twitter cards, and Schema.org JSON-LD (type: AutoRepair). Business details (address, phone, hours) live here. The default OG image is `/logo.jpg` (absolute URL via `Astro.site`).

### Design System

- **Industrial/bold aesthetic**: dark background `#1A1A1A`, accent `#F5A623` (amber/orange)
- **Fonts**: Bebas Neue for headings, Barlow Condensed for body (loaded via Google Fonts in `global.css`)
- **Custom theme tokens** defined in `src/styles/global.css` using Tailwind v4 `@theme` directive (e.g., `--color-primary`, `--color-surface`)
- **Reusable CSS classes**: `.btn-bold`, `.bold-card`, `.bold-label`, `.service-card-3d` — defined in `global.css`, not as Tailwind utilities
- No custom cursor — uses default browser cursor

### Pages

5 main pages (index, servicios, galeria, contacto, blog index) + 5 blog article pages. Blog articles are standalone `.astro` files (no content collections or markdown).

### Gallery

- Real workshop photos in `public/galeria/` (7 JPGs, compressed to ~270KB each at 1400px max)
- `src/pages/galeria.astro` renders them in a grid with a **vanilla JS lightbox** (no dependencies)
- Lightbox: click to open, ‹/› arrows or keyboard arrow keys to navigate, Escape or click outside to close

### Assets

- `public/LOGO_EPROM_.png` — logo with transparent background (used in Navbar and hero)
- `public/logo.jpg` — logo on orange background (used as favicon and OG social share image)
- `public/galeria/` — 7 compressed workshop photos

### Business Contact Info

- Phone: 971 906 236 / WhatsApp: 625 086 015
- Address: Carrer de Joan Fuster i Ortells, 24, Palma
- Hours: Mon-Thu 08:00-17:00, Fri 08:00-14:00

When editing contact details, update in: SEO.astro (JSON-LD), Footer.astro, contacto.astro, WhatsAppButton.astro, and Navbar.astro.

## Key Conventions

- Inline styles are used heavily alongside Tailwind classes — this is intentional, not a mistake
- Mobile-first responsive: breakpoints handled via `@media` in global.css and Tailwind's `md:` / `lg:` prefixes
- Node >= 22.12.0 required (see `engines` in package.json)
- Gallery images added to `public/` must be committed to git — they are not auto-tracked
- Compress new images before committing: `sips -Z 1400 -s format jpeg -s formatOptions 82 image.jpg --out image.jpg`
