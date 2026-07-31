# data.acousticobservatory.org — Hugo Static Site Migration

This repository contains a full migration of [acousticobservatory.org](https://acousticobservatory.org/) from a hosted WordPress site to a [Hugo](https://gohugo.io/) static site, targeting deployment on [Netlify](https://www.netlify.com/).

The static site lives in the [`landing_site/`](./landing_site/) directory.

---

## What was done

### Migration approach

The original site was downloaded via a Jetpack backup (full WordPress export including media uploads). All page content, images, and structure were extracted and converted to Hugo-compatible markdown and templates.

### Design system

A clean CSS-only design system was built from scratch in [`landing_site/static/css/design-system.css`](./landing_site/static/css/design-system.css), replacing the original WordPress theme. Key principles:

- **No preprocessors** — plain CSS with custom properties
- **Semantic over classed** — element selectors and BEM-style component classes used sparingly
- **Responsive** — flexbox/grid layouts, fluid typography, mobile-first breakpoints

### Content

- All WordPress pages converted to Hugo markdown with YAML front matter
- **85 site/point pages** converted from raw WordPress block HTML to clean markdown tables, with structured front matter: `site_id`, `point_ids`, `ecoregion`, `summary`, and `images` for search indexing and cross-linking
- Image galleries use a reusable `{{< image-mosaic >}}` shortcode

### Templates & layouts

Hugo templates in [`landing_site/layouts/`](./landing_site/layouts/) cover:

| Layout | Purpose |
|---|---|
| `_default/baseof.html` | Base shell with header/footer |
| `index.html` | Homepage with hero carousel and section cards |
| `sites/list.html` | Filterable site directory with ecoregion facets |
| `ecoregions/single.html` | Ecoregion tile grid |
| `partials/tile-card.html` | Shared card partial (image + title + summary) |
| `partials/image-mosaic.html` | CSS-columns photo mosaic |
| `partials/header.html` | Responsive navigation header |
| `partials/footer.html` | Footer with logo grid and links |

### Search / filtering

The site directory (`/sites/`) uses lightweight client-side JS filtering:
- URL parameter `?q=` pre-fills the search on load
- Filters by site name, ecoregion, and region
- Ecoregion pages link directly to pre-filtered site lists

### Contact form

The contact page uses [Netlify Forms](https://docs.netlify.com/forms/setup/) — no server required. The form is marked with `data-netlify="true"`.

### Assets

All binary assets (images, PDFs, media) in `landing_site/static/` are tracked with **Git LFS** via [`.gitattributes`](./.gitattributes). WordPress-generated thumbnail variants (1412 files, `filename-NNNxNNN.ext` pattern) were removed; only original/highest-resolution files are kept.

Link integrity is verified with [htmltest](https://github.com/wjdp/htmltest) — see [`landing_site/.htmltest.yml`](./landing_site/.htmltest.yml).

---

## Prerequisites

- [Hugo extended](https://gohugo.io/installation/) v0.110+
- [Git LFS](https://git-lfs.com/) (for image assets)
- [Node.js](https://nodejs.org/) (optional — used for batch content scripts only)

---

## Running locally

```bash
cd landing_site
hugo server --renderToMemory --disableFastRender
```

The site will be available at [http://localhost:1313](http://localhost:1313).

> **Note:** If CSS changes aren't reflecting, restart the server — Hugo's fast-render mode can cache stale CSS in development.

---

## Building for production

```bash
cd landing_site
hugo build
```

Output is written to `landing_site/public/`. This directory is ignored by git (Netlify builds from source).

---

## Deploying to Netlify

1. Connect the repository to Netlify
2. Set **Base directory** to `landing_site`
3. Set **Build command** to `hugo`
4. Set **Publish directory** to `landing_site/public`

Netlify will automatically handle form submissions from the contact page.

---

## Link checking

```bash
cd landing_site
hugo build
htmltest  # reads .htmltest.yml config
```

---

## Repository structure

```
.
├── .gitattributes          # Git LFS tracking rules for binary assets
├── .gitignore
├── README.md               # This file
└── landing_site/
    ├── hugo.toml           # Hugo site configuration
    ├── .htmltest.yml       # htmltest link checker config
    ├── content/            # All site pages as markdown
    │   ├── sites/          # 85 recording site pages
    │   ├── ecoregions/     # Ecoregion listing
    │   ├── data/           # Data access pages
    │   └── ...
    ├── layouts/            # Hugo HTML templates
    │   ├── _default/
    │   ├── partials/
    │   └── shortcodes/
    └── static/
        ├── css/
        │   └── design-system.css
        └── wp-content/
            └── uploads/    # Migrated media assets (tracked via LFS)
```
