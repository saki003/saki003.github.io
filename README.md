# rubenmora.github.io — academic site

Static personal academic site for Ruben Mora, MD. Plain HTML + CSS, no build step, no dependencies.

```
index.html      # the whole site (single page, anchor nav)
styles.css      # all styling; light + dark, responsive, print stylesheet
assets/
  RubenMoraCV.pdf
.nojekyll       # tells GitHub Pages to serve files as-is
```

## Preview locally

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000

## Deploy to GitHub Pages

1. Create a **public** repo on GitHub named exactly `<your-username>.github.io`
   (e.g. `rubenmora.github.io`). That name gives you `https://<username>.github.io`
   with no subpath.
2. From this folder:

```bash
git init && git add -A && git commit -m "Initial site" && git branch -M main
```

3. Point it at the repo and push:

```bash
git remote add origin https://github.com/<your-username>/<your-username>.github.io.git && git push -u origin main
```

4. In the repo: **Settings → Pages → Source: Deploy from a branch → `main` / `(root)`**.
   Live in about a minute.

### Custom domain (optional)

Buy a domain, add a `CNAME` file containing just the domain (e.g. `rubenmora.md`), and set
the DNS records GitHub lists under Settings → Pages.

## Before you share the link — fill these in

Search `index.html` for `TODO` and `YOUR`:

- **`SITE_URL`** — appears 5×. Replace every instance with your live origin, no trailing
  slash (e.g. `https://rubenmora.github.io`). These drive the link preview card on X,
  LinkedIn, and Slack. `sed -i '' 's|SITE_URL|https://rubenmora.github.io|g' index.html`
- **Profile links** in the hero — Google Scholar ID, ORCID, X handle. Delete any you
  don't want; the PubMed link is a live author search and already works.
- **`assets/og-card.png`** — the social preview image, **1200×630 px**. Not included.
  Without it, X and LinkedIn fall back to a plain text card. A clean option: your name,
  credential, and one line of research focus on a solid background. A headshot works too.

## Updating publications

Publication entries live in `index.html` under `<section id="publications">`. Each is one
`<li>` with three lines — `.title`, `.authors`, `.venue`. Copy an existing one and edit.
Numbering is automatic (CSS counters), so order is the only thing you control.

When a paper moves from **Under review** to published, move its `<li>` into the
peer-reviewed list, add the DOI link, and bump the count in the hero stats block
(`.hero-stats`).

Replace the CV with `cp /path/to/new-cv.pdf assets/RubenMoraCV.pdf`.
