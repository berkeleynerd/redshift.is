# redshift.is

The home page for Redshift ehf., built with [smol](https://github.com/berkeleynerd/smol) —
a standard-library Go static-site generator producing self-contained, no-JS XHTML.

This repository uses two branches:

- **`main`** (this branch): the smol source — `smol.json`, `content/`, and the
  `redshift-xhtml` theme under `themes/`.
- **`pages`**: only the generated output that Codeberg Pages serves
  (`index.html` plus `.domains`, which declares the custom domain).

## Build

```sh
smol build --force        # writes public/index.html
smol check --mode flat-xhtml-v1 public
```

`public/` is git-ignored here; it is the artifact deployed to `pages`.

## Deploy

```sh
smol build --force
git switch pages
cp public/index.html index.html
git add index.html
git commit -m "Publish: <what changed>"
git push origin pages
git switch main
```

Codeberg Pages serves the root of the `pages` branch. The `.domains` file lists
`redshift.is` (canonical) and `www.redshift.is`; point DNS at Codeberg Pages per
their documentation to activate the custom domain.
