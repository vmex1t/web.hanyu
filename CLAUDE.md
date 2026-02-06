# CLAUDE.md — AI Assistant Guide for web.hanyu

## Project Overview

This is the **marketing landing page** for [Hanyu](https://hanyu.app), an AI-powered Chinese language learning app. It is a static site built with **Astro** that showcases the app's features and collects waitlist sign-ups via a Google Forms integration.

## Tech Stack

- **Framework**: Astro 5.x (static site generation)
- **Language**: TypeScript (strict mode)
- **Styling**: Vanilla CSS with scoped component styles + a global stylesheet
- **Package Manager**: npm
- **No runtime dependencies** beyond Astro itself

## Repository Structure

```
web.hanyu/
├── astro.config.mjs          # Astro config (site: hanyu.app, output: static)
├── tsconfig.json              # Extends astro/tsconfigs/strict
├── package.json               # Only dependency is astro
├── public/                    # Static assets served as-is
│   ├── favicon.svg
│   ├── fonts/                 # Custom font (NBI International Pro CG)
│   └── screenshots/           # App screenshot PNGs for feature sections
└── src/
    ├── components/            # Astro components (one per landing page section)
    │   ├── Navbar.astro       # Sticky nav with scroll shadow effect
    │   ├── Hero.astro         # Hero with phone mockups
    │   ├── Features.astro     # Feature showcase (Dictionary, Writing, Practice)
    │   ├── Screenshots.astro  # Horizontal scrolling gallery
    │   ├── HowItWorks.astro   # 3-step onboarding flow
    │   ├── FAQ.astro          # Expandable accordion
    │   ├── CTA.astro          # Waitlist form (Google Forms integration)
    │   └── Footer.astro       # Footer with social/legal links
    ├── layouts/
    │   └── Layout.astro       # Base HTML layout (head, meta, OG tags, font preload)
    ├── pages/                 # File-based routing
    │   ├── index.astro        # Home (composes all section components)
    │   ├── privacy.astro      # Privacy policy
    │   └── terms.astro        # Terms of service
    └── styles/
        └── global.css         # CSS reset, custom properties, utility classes
```

## Commands

```bash
npm run dev       # Start local dev server
npm run build     # Production build (outputs to dist/)
npm run preview   # Preview the production build locally
```

There are **no tests, linters, or formatters** configured in this project.

## Architecture & Patterns

### Astro Components

Each `.astro` file follows this structure:

```astro
---
// Frontmatter: TypeScript for data, imports, props
const items = [...]
---

<!-- Template: HTML markup with JSX-like expressions -->
<section>
  {items.map(item => <div>{item.name}</div>)}
</section>

<script>
  // Client-side JS (runs in browser, not at build time)
</script>

<style>
  /* Scoped CSS (automatically scoped to this component) */
</style>
```

### Page Composition

`index.astro` composes the landing page from section components. Each component is self-contained with its own markup, styles, and (where needed) client-side JavaScript. The `Layout.astro` wrapper provides the `<html>`, `<head>`, and global styles.

### Styling Conventions

- **Global CSS custom properties** defined in `src/styles/global.css` under `:root`
- **Color palette**: `--color-primary: #0061FF`, `--color-bg: #FFFFFF`, `--color-text: #1E1919`, `--color-text-secondary: #637381`, `--color-bg-alt: #F7F5F2`
- **Typography**: Uses `clamp()` for fluid scaling (e.g., `--size-h1: clamp(2.5rem, 5.5vw, 3.75rem)`)
- **Layout**: Max width `--max-width: 1200px`, section padding via `--section-padding`
- **Responsive breakpoints**: 768px (tablet), 480px (mobile)
- **Naming**: kebab-case class names, BEM-like modifiers (e.g., `.feature-row--reverse`)
- **Scoped styles**: Each component uses `<style>` for component-specific CSS; global styles go in `global.css`

### JavaScript

Minimal vanilla JS used only for:
- **Scroll-reveal**: IntersectionObserver adds `.visible` to `.reveal` elements
- **Navbar shadow**: Adds shadow class on scroll
- **Waitlist form**: Client-side validation, honeypot spam protection, submits to Google Forms via `fetch` with `no-cors`, shows result in a modal overlay

### Data Patterns

Content data (features list, FAQ items, etc.) is defined as arrays in component frontmatter and iterated with `.map()`. There is no CMS, database, or external API beyond the Google Forms endpoint.

## Key Conventions

- **Static output only** — no SSR, no server-side logic
- **Single dependency** — only Astro; avoid adding unnecessary packages
- **Component-per-section** — each landing page section is its own `.astro` file
- **Semantic HTML** — use proper heading hierarchy, `<section>`, `<nav>`, `<main>`, etc.
- **Accessibility** — use `aria-hidden` for decorative elements, proper `<label>`/`<fieldset>` for forms, focus-visible styles for interactive elements
- **Mobile-first** — base styles target mobile, `@media` queries add tablet/desktop layouts
- **Custom font** — "NBI International Pro CG" is preloaded from `/fonts/`; fallback to `system-ui, -apple-system, sans-serif`

## Deployment

- Configured for **static hosting** (any CDN/host works)
- `.vercel` is in `.gitignore`, suggesting Vercel as the intended deployment target
- Build output goes to `dist/`

## External Integrations

- **Google Forms**: Waitlist form submits to a Google Forms endpoint (entry IDs in `CTA.astro`)
- **No analytics SDK** in source (privacy policy references Google Analytics)
