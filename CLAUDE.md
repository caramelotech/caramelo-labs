# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Caramelo Labs is a documentation site built with [Astro](https://astro.build/) and the [Starlight](https://starlight.astro.build/) theme. It is published to GitHub Pages at `https://caramelotech.com.br/caramelo-labs`. The repo has no backend or build-time tests.

## Commands

```bash
npm install        # install dependencies
npm run dev        # dev server at localhost:4321
npm run build      # production build (outputs to dist/)
npm run preview    # preview the production build locally
```

## Architecture

```
src/content/docs/   # Markdown/MDX pages - each file becomes a route
src/content/config.ts  # Zod schema for the docs collection
src/styles/custom.css  # Starlight CSS overrides
astro.config.mjs    # Astro + Starlight configuration (base path, locale, sidebar)
examples/           # Code examples, exercises, and project prompts (not rendered by Astro)
```

Content is routed automatically by filename. The sidebar order is controlled by the `sidebar.order` frontmatter field. Subfolders inside `src/content/docs/` group notes by topic and become URL segments.

## Content conventions

All notes are written in **Portuguese (pt-BR)**. Every `.md` file in `src/content/docs/` requires this frontmatter:

```yaml
---
title: "Título da nota"
description: "Descrição breve"
lastUpdated: 2026-01-01
sidebar:
  order: 1
tags: ["tag1", "tag2"]
---
```

- Do not repeat the `title` as an `# h1` inside the body - Starlight renders it automatically.
- Use `##` and `###` for sections.
- The `date` field is also accepted (coerced to Date by the schema).

**`sidebar.order` é sequencial por diretório**, não global. A ordem entre seções já é controlada pelo array `sidebar` em `astro.config.mjs`. Dentro de cada pasta, numere os arquivos a partir de 1.

Para adicionar uma nova seção superior (ex: `nova-categoria/`):

1. Crie o diretório em `src/content/docs/nova-categoria/`
2. Adicione um arquivo `index.md` como página de entrada
3. Adicione uma entrada `autogenerate` em `astro.config.mjs`:
   ```javascript
   {
     label: "Título da Seção",
     autogenerate: { directory: "nova-categoria" },
   }
   ```

## Deployment

The GitHub Actions workflow at `.github/workflows/deploy.yml` runs `npm run build` and deploys `dist/` to GitHub Pages on every push to `main`. The `base` in `astro.config.mjs` must stay as `/caramelo-labs` to match the Pages sub-path.
