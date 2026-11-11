# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A personal online resume built on the "Orbit" Jekyll theme (forked from `sharu725/online-cv`), deployed via GitHub Pages from `master`. There is no test suite or linter; validation means building the site and checking the rendered pages.

## Commands

```bash
bundle install                 # Ruby version pinned in .tool-versions (2.7.5)
bundle exec jekyll serve       # http://localhost:4000 (host/port set in _config.yml)
bundle exec jekyll build       # build into _site/ — use to catch YAML/Liquid errors
docker-compose up              # alternative: serve with livereload in jekyll/builder:4.0 container
```

`_config.yml` changes (e.g. `theme_skin`) require restarting `jekyll serve`.

This repo is a fork; upstream theme updates are pulled by rebasing onto `sharu725/online-cv` `master`. Personal changes should stay confined to `_data/data.yml`, `_config.yml` and `assets/images/` so rebases only conflict there.

## Architecture

- **All resume content lives in `_data/data.yml`.** Edit content there, not in HTML. Top-level keys (`sidebar`, `career-profile`, `education`, `experiences`, `projects`, `skills`, `footer`, and optionally `oss-contributions`, `publications`, `certifications`, `recommendations`) each map to a partial in `_includes/` that reads `site.data.data.<key>` and renders nothing if the key is absent. A YAML syntax error breaks the whole build.
- `details`/`description` fields are passed through `markdownify`, so Markdown is supported there.
- **Two pages share the same partials:** `index.html` (layout `default`) and `print.html` (layout `print`, served at `/print`, a printer-friendly version). Section order is defined in each page file, and they must be kept in sync manually.
- `sidebar.education: True` moves education into the sidebar instead of the main column (handled by `unless` in both page files).
- **Styling:** `assets/css/main.scss` imports the skin chosen by `theme_skin` in `_config.yml` (`_sass/skins/_<name>.scss`, which sets color variables) then `_sass/_default.scss`. `_print.scss` holds print-specific rules.
- The PDF button opens `/print#save`, which triggers the browser print dialog ("Save as PDF"); PDF output is styled by `_print.scss` and the `print` layout.
- `assets/plugins/` contains vendored Bootstrap and Font Awesome — don't edit.
- The site is served from `https://jb-delafosse.github.io/online-cv/`, so `baseurl` is `/online-cv`; asset and page links in templates must be prefixed with `{{ site.baseurl }}`. Locally it's served at `http://localhost:4000/online-cv/`.
