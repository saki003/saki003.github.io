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

### Custom domain

Live at **https://saki003.github.io**. The plan is to move to `rubenmoramd.com`.

**Order matters.** Do not commit a `CNAME` file before the domain resolves — GitHub will
301 every request to the dead domain and the site goes offline entirely. Correct sequence:

1. Register the domain.
2. At the registrar, create four `A` records for the apex (`@`) pointing to
   `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`,
   and a `CNAME` for `www` → `saki003.github.io`.
3. Wait for DNS to resolve (`dig +short rubenmoramd.com` returns those IPs).
4. Only then: `echo rubenmoramd.com > CNAME`, commit, push.
5. Repo → Settings → Pages → set the custom domain, then tick **Enforce HTTPS** once the
   certificate is issued (can take up to an hour).
6. Update the four absolute URLs in `index.html` (`og:url`, `og:image`, `twitter:image`,
   `canonical`) and the JSON-LD `url` from `saki003.github.io` to the new domain.

### Regenerating the social card

`assets/og-card.png` (1200×630) is rendered from `assets/og-card.svg`:

```bash
cd assets && cp og-card.svg r.svg && qlmanage -t -s 1200 -o . r.svg && sips -c 630 1200 r.svg.png --out og-card.png && rm r.svg r.svg.png
```

The SVG is authored on a 1200×1200 canvas with the card centred so the crop lands exactly.
Note: XML comments cannot contain `--`, which silently breaks the render.

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
