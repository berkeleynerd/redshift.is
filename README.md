# redshift.is

The home page for Redshift ehf., built with [smol](https://github.com/berkeleynerd/smol) —
a standard-library Go static-site generator producing self-contained, no-JS XHTML.

This repository uses two branches:

- **`main`** (this branch): the smol source — `smol.json`, `content/`, and the
  `redshift-xhtml` theme under `themes/`.
- **`gh-pages`**: only the generated output that GitHub Pages serves —
  `index.html`, `404.html`, `CNAME` (the custom domain), and `.nojekyll`
  (tells GitHub to serve the files as-is, skipping Jekyll).

## Build

```sh
smol build --force                  # writes public/index.html and public/404.html
smol check --mode flat-xhtml-v1 public
```

`public/` is git-ignored here; it is the artifact deployed to `gh-pages`.

## Deploy

```sh
smol build --force
git switch gh-pages
git pull --ff-only                  # in case GitHub touched CNAME
cp public/index.html public/404.html .
git add index.html 404.html
git commit -m "Publish: <what changed>"
git push origin gh-pages
git switch main
```

Leave `CNAME` and `.nojekyll` in place on `gh-pages`; only the built HTML
changes between deploys.

## Hosting

GitHub Pages serves the root of the `gh-pages` branch at the custom domain
**https://redshift.is/** (with `www.redshift.is` redirecting to it). The
`CNAME` file declares the domain; it is registered under the repository's
**Settings → Pages**, with a Let's Encrypt certificate and **Enforce HTTPS**
enabled.

DNS (at the domain registrar) points the domain at GitHub Pages:

```
redshift.is.       A      185.199.108.153
redshift.is.       A      185.199.109.153
redshift.is.       A      185.199.110.153
redshift.is.       A      185.199.111.153
redshift.is.       AAAA   2606:50c0:8000::153
redshift.is.       AAAA   2606:50c0:8001::153
redshift.is.       AAAA   2606:50c0:8002::153
redshift.is.       AAAA   2606:50c0:8003::153
www.redshift.is.   CNAME  berkeleynerd.github.io.
```

Email (Proton Mail MX/SPF/DKIM records) is configured separately on the same
zone and is unaffected by the Pages setup.
