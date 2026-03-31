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

## Architecture

- **Astro 6** static site generator with file-based routing (`src/pages/`)
- **Tailwind CSS v4** via Vite plugin (not PostCSS) — configured in `astro.config.mjs`
- **Sitemap** auto-generated via `@astrojs/sitemap`
- Site URL: `https://electromecanicaeprom.es` with base path `/Electromecanica-Eprom`

### Layout & Components

All pages use `src/layouts/Layout.astro` which wraps content with Navbar, Footer, WhatsAppButton, and a custom cursor script. Every page must pass `title` and `description` props to Layout.

`src/components/SEO.astro` handles meta tags, Open Graph, Twitter cards, and Schema.org JSON-LD (type: AutoRepair). Business details (address, phone, hours) live here.

### Design System

- **Industrial/bold aesthetic**: dark background `#1A1A1A`, accent `#F5A623` (amber/orange)
- **Fonts**: Bebas Neue for headings, Barlow Condensed for body (loaded via Google Fonts in `global.css`)
- **Custom theme tokens** defined in `src/styles/global.css` using Tailwind v4 `@theme` directive (e.g., `--color-primary`, `--color-surface`)
- **Reusable CSS classes**: `.btn-bold`, `.bold-card`, `.bold-label`, `.service-card-3d` — defined in `global.css`, not as Tailwind utilities
- Custom cursor (desktop only) with accent color dot

### Pages

5 main pages (index, servicios, galeria, contacto, blog index) + 5 blog article pages. Blog articles are standalone `.astro` files (no content collections or markdown).

### Business Contact Info

- Phone: 971 906 236 / WhatsApp: 625 086 015
- Address: Carrer de Joan Fuster i Ortells, 24, Palma
- Hours: Mon-Thu 08:00-17:00, Fri 08:00-14:00

When editing contact details, update in: SEO.astro (JSON-LD), Footer.astro, contacto.astro, WhatsAppButton.astro, and Navbar.astro.

## Key Conventions

- Inline styles are used heavily alongside Tailwind classes — this is intentional, not a mistake
- Mobile-first responsive: breakpoints handled via `@media` in global.css and Tailwind's `md:` / `lg:` prefixes
- Node >= 22.12.0 required (see `engines` in package.json)
