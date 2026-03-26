# AGENTS.md

Guidelines for AI coding agents working in this repository.

## Project Overview

Jekyll 3.10 static blog using the Minima theme, hosted on GitHub Pages at stevenhostetler.com. Ruby/Bundler for dependency management. No Node.js, no TypeScript, no JavaScript source files.

## Build & Serve Commands

```sh
bundle install              # install Ruby dependencies
bundle exec jekyll serve    # local dev server at http://localhost:4000
bundle exec jekyll build    # build site to _site/
```

## Testing & Linting

None configured. The primary verification is a clean build:

```sh
bundle exec jekyll build
```

If the build completes without errors, the site is valid. Check `_site/` output if needed, but never edit it directly.

## Content Conventions

### Blog Posts (`_posts/`)

- Filename: `YYYY-MM-DD-slug.markdown` (e.g. `2026-03-25-ai-coding-agent.markdown`)
- Use `.markdown` extension (not `.md`) for posts
- Required front matter:

```yaml
---
layout: post
title: "Post Title Here"
date: YYYY-MM-DD HH:MM:SS -0000
categories: personal
permalink: /:categories/:title/
---
```

- Valid categories: `personal`, `ai`

### Pages (root directory)

- Use `.md` extension
- Standard front matter:

```yaml
---
layout: page
title: Page Title
permalink: /page-url/
---
```

### Liquid Templates (`_includes/`)

- Use Liquid syntax: `{% if %}`, `{{ variable }}`, `{% for %}`
- Custom includes go in `_includes/`
- Layout overrides go in `_layouts/`

## Site Configuration (`_config.yml`)

- Controls title, description, URL, author, theme, and plugins
- `header_pages` determines navigation menu order
- Active plugins: `jekyll-feed`, `jekyll-sitemap`, `jekyll-seo-tag`
- Only GitHub Pages-compatible gems/plugins are allowed

## Deployment

Push to `main`. GitHub Pages builds and deploys automatically. The `CNAME` file must contain `stevenhostetler.com`.

## Constraints

- Never edit `_site/` directly — it is build output
- Do not add plugins or gems not supported by the `github-pages` gem
- Preserve `.gitignore` entries: `_site/`, `.sass-cache/`, `.jekyll-metadata/`
- Commit `Gemfile.lock` for reproducible builds
- Posts and pages should be valid Markdown with kramdown syntax
