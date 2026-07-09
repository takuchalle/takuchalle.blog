# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal blog (takuchalle.blog) built with Hugo (extended) using the [Blowfish](https://github.com/nunocoracao/blowfish) theme, deployed to Cloudflare Workers as a static site.

## Commands

```sh
# Initial setup: theme is a git submodule and must be fetched
git submodule update --init --recursive

# Local dev server with drafts/future posts visible, live reload
hugo server -D -F

# Production build (same command Cloudflare runs via wrangler.jsonc)
hugo --minify

# New post
hugo new posts/<category>/<slug>.md
```

There is no test suite, linter, or CI configuration in this repo — the only correctness check is that `hugo --minify` builds cleanly and the site looks right in `hugo server`.

## Architecture

- **Theme as submodule**: All layout/rendering logic lives in `themes/blowfish` (a separate upstream repo, pinned via `.gitmodules`, tracking its `main` branch). This repo only overrides/extends it — never edit files under `themes/blowfish` directly; put overrides in this repo's own `layouts/`.
- **Split config**: Hugo config is not a single file but `config/_default/*.toml`, each with a distinct responsibility:
  - `hugo.toml` — site-wide settings (baseURL, taxonomies, pagination, outputs, related-content weighting)
  - `languages.en.toml` — language/site title and author metadata (bio, links, contact)
  - `markup.toml` — goldmark/highlight settings required by the theme (do not remove `unsafe = true` or the passthrough math delimiters without checking theme compatibility)
  - `menus.en.toml` — header/footer nav entries
  - `params.toml` — Blowfish theme parameters (appearance/color scheme, per-section layout toggles for homepage/article/list/taxonomy/term)
- **Content layout**: Posts live under `content/posts/<category>/<slug>.md` (categories currently: `book`, `diary`, `programming/{flutter,git,rust,vim,zig}`). Frontmatter is YAML (`---`), even though `archetypes/default.md` generates TOML (`+++`) — follow the existing YAML style used by real posts, not the archetype, when creating new content. Typical frontmatter fields: `title`, `description`, `date`, `categories`, `tags`, `author`, `images` (OGP image path under `/img/ogp/`).
- **Shortcodes**: Custom shortcodes in `layouts/shortcodes/` extend the theme — `card.html` embeds a Hatena blog card via iframe, `gphoto.html` embeds a Google Drive/Photos image by file id.
- **Deployment**: `wrangler.jsonc` configures a Cloudflare Workers static-assets deployment — it runs `hugo --minify` and serves the generated `public/` directory, with a custom 404 page handler.
