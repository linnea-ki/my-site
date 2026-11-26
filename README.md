# my-site

Static research website: plain HTML and CSS, no framework, no build step.

```
my-site/
├── index.html        # all content (About, Software, Team, Publications, CV, Contact)
├── style.css         # all styling; colours are CSS variables at the top
├── .nojekyll         # tells GitHub Pages to serve files as-is
└── assets/
    ├── favicon.svg
    ├── cv.pdf        # ← add your CV here (linked from the CV section)
    └── team/         # ← optional team photos
```

## Editing

Every placeholder is in `[square brackets]`, and every block to fill in has an
`<!-- EDIT: ... -->` comment above it. Search `index.html` for `[`, `EDIT` and
`YOUR-USERNAME`.

- **Software / team / publications:** copy an existing `<article>` or
  `<li class="pub">` block and change the text.
- **Team photos:** put `name.jpg` in `assets/team/` and replace
  `<span class="avatar">..</span>` with
  `<img class="avatar" src="assets/team/name.jpg" alt="Photo of Name">`.
- **Colours:** change `--accent` (and its dark-mode value) in `style.css`.

Preview locally by opening `index.html` in a browser, or
`python -m http.server -d my-site` and go to http://localhost:8000.

## Deploying to GitHub Pages

All paths are relative, so the site works both at `https://USERNAME.github.io/`
and at `https://USERNAME.github.io/REPO/`.

**Option A: the site is the whole repository**

1. Create a repository (named `USERNAME.github.io` for a personal root site,
   or any name for a project site).
2. Push the *contents* of `my-site/` to the root of the `main` branch.
3. Settings → Pages → Source: *Deploy from a branch* → `main` / `/ (root)`.

**Option B: the site lives in a folder of a code repository**

GitHub Pages can only serve from `/` or `/docs`, so either rename `my-site` to
`docs` and choose `main` / `/docs`, or keep the name and deploy with a GitHub
Actions workflow (`.github/workflows/pages.yml`):

```yaml
name: Deploy site
on:
  push:
    branches: [main]
permissions:
  contents: read
  pages: write
  id-token: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - uses: actions/checkout@v7
      - uses: actions/configure-pages@v6
      - uses: actions/upload-pages-artifact@v5
        with:
          path: my-site
      - id: deployment
        uses: actions/deploy-pages@v5
```

Then set Settings → Pages → Source: *GitHub Actions*.
