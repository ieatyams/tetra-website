# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Tetra (tetra.consulting) is a static marketing site for a finance consulting firm. Pure HTML/CSS/JS with no build step, no frameworks, and no package manager.

## Deployment

- **Host**: Cloudflare Workers static asset deployment
- **Config**: `wrangler.jsonc` serves the root directory as static assets
- **Deploy**: `npx wrangler deploy` (or via Cloudflare dashboard)
- **Domain**: tetra.consulting

There is no build step. Edits to HTML files are deployed directly.

## Architecture

Each page is a self-contained HTML file with inline `<style>` and `<script>` blocks. There is no shared CSS file — design tokens and component styles are duplicated per page.

### Pages

| Path | Purpose | Notes |
|---|---|---|
| `/index.html` | Homepage | Full nav with scroll effect, hero, sections, CTA |
| `/submitrequest/index.html` | Contact form | `noindex` meta tag, Formspree async submit |
| `/privacy/index.html` | Privacy policy | Static content page |

### Design System (inline in each page)

- **Color tokens**: `--copper`, `--charcoal`, `--parchment`, `--steel`, `--yale` and variants
- **Fonts**: Montserrat (headings), Source Sans Pro (body), Space Mono (labels/mono)
- **Components**: `.nav`, `.btn`, `.section`, `.footer`, `.reveal` (scroll animation)
- **Breakpoint**: 768px for mobile layout
- **Motion**: `prefers-reduced-motion` respected on all pages

### Key Integrations

- **Formspree** (`https://formspree.io/f/xreaobjb`): Handles form submissions on `/submitrequest/`
- **Google Fonts**: Loaded via `<link>` tags in `<head>`

## Important Patterns

- When adding a new page, replicate the full `<head>` block (meta, favicons, fonts), nav, footer, and JS (scroll reveal, mobile toggle) from an existing page. Use `submitrequest/index.html` as the simpler template.
- Footer links must be updated on **all three pages** when adding a new link — there is no shared template.
- The nav on the homepage has a transparent-to-solid scroll effect; inner pages use a static solid nav.
- SEO files (`sitemap.xml`, `robots.txt`) must be manually updated when adding public pages.
