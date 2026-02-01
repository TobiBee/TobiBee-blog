# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Astro-based blog using the AstroPaper theme. It's a personal blog for Tobias Billinger at tobibee.dev.

## Commands

```bash
pnpm install          # Install dependencies
pnpm run dev          # Start dev server at localhost:4321
pnpm run build        # Type-check, build to ./dist/, generate pagefind search index
pnpm run preview      # Preview production build locally
pnpm run format       # Format with Prettier
pnpm run format:check # Check formatting
pnpm run lint         # Run ESLint
pnpm run sync         # Generate TypeScript types for Astro modules
```

## Architecture

### Content System
- Blog posts are Markdown files in `src/data/blog/`
- Content schema defined in `src/content.config.ts` using Zod validation
- Posts prefixed with `_` in folder names don't affect URL paths
- Draft posts (`draft: true`) only visible in dev mode
- Scheduled posts have a 15-minute margin before becoming visible

### Post Frontmatter Schema
```yaml
author: string          # defaults to SITE.author
pubDatetime: date       # required
modDatetime: date       # optional
title: string           # required
slug: string            # optional, auto-generated from filename
featured: boolean       # optional
draft: boolean          # optional
tags: string[]          # defaults to ["others"]
ogImage: string/image   # optional
description: string     # required
canonicalURL: string    # optional
timezone: string        # optional, IANA format
```

### Key Configuration Files
- `src/config.ts` - Site-wide settings (title, author, pagination, features)
- `src/constants.ts` - Social links and share buttons
- `astro.config.ts` - Astro configuration, remark plugins, Shiki code highlighting

### Layout Hierarchy
- `Layout.astro` - Base HTML with SEO meta, structured data, theme handling
- `Main.astro` - Page wrapper with header/footer
- `PostDetails.astro` - Blog post layout with sharing, tags, navigation
- `AboutLayout.astro` - About page layout

### Styling
- Tailwind CSS v4 with custom theme in `src/styles/global.css`
- CSS variables for theming: `--background`, `--foreground`, `--accent`, `--muted`, `--border`
- Light/dark mode via `data-theme` attribute on `<html>`

### Utilities (`src/utils/`)
- `getSortedPosts.ts` - Filters and sorts posts by date
- `postFilter.ts` - Filters out drafts and future posts
- `slugify.ts` - URL-safe slug generation
- `generateOgImages.ts` - Dynamic OG image generation with Satori/resvg

### Dynamic Routes
- `/posts/[...page].astro` - Paginated post listing
- `/posts/[...slug]/index.astro` - Individual post pages
- `/posts/[...slug]/index.png.ts` - Dynamic OG images per post
- `/tags/[tag]/[...page].astro` - Posts filtered by tag

## Code Style

- ESLint with TypeScript and Astro plugins
- `no-console` rule enforced
- Prettier for formatting with Astro and Tailwind plugins
- Uses `@/` path alias for `src/` imports
