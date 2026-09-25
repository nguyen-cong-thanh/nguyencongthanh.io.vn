# nguyencongthanh.io.vn

Static site built with [Hugo](https://gohugo.io/) (extended v0.166.0) and the [LoveIt](https://github.com/dillonzq/LoveIt) theme, deployed to GitHub Pages.

## Clone

The theme is managed as a git submodule:

```sh
git clone --recurse-submodules git@github.com:nguyen-cong-thanh/nguyencongthanh.io.vn.git
# or, for an existing clone:
git submodule update --init --recursive
```

## Run locally

Only Docker is required. The following runs `hugo server` (including drafts) at http://localhost:1313/:

```sh
docker compose up
```

The container runs as UID/GID `1000:1000` so generated files belong to the current user. If your UID/GID differ, export `UID`/`GID` before running.

Build once into `public/`:

```sh
docker compose run --rm hugo --gc --minify
```

## Bilingual posts

Vietnamese is the default language (served at `/`); English is served under `/en/`. Each post is a directory with one file per language; Hugo links the two files in the same directory as translations of each other:

```
content/posts/<slug>/index.vi.md
content/posts/<slug>/index.en.md
```

Create a new post:

```sh
docker compose run --rm hugo new content posts/<slug>/index.vi.md
docker compose run --rm hugo new content posts/<slug>/index.en.md
```

New posts have `draft = true`; set it to `false` to publish.

## Series

A series groups ordered posts (part 1, part 2, ...). Add the `series` taxonomy and a part number to the front matter of each language file:

```toml
series = ["Python cơ bản"]   # in index.vi.md
series = ["Python basics"]   # in index.en.md
series_weight = 2            # part number, same in both files
```

- `/series/` (and `/en/series/`) lists all series; each series page lists its parts in `series_weight` order.
- Every post in a series shows a box above its content with its part number, all parts of the series, and links to the previous and next parts.

LoveIt has no series support, so this is implemented in the site:

- [layouts/_partials/series.html](layouts/_partials/series.html): the series box.
- [layouts/posts/single.html](layouts/posts/single.html): a copy of LoveIt's template with one added call to the series partial. After updating the theme, re-apply that change on top of the new `themes/LoveIt/layouts/posts/single.html`.
- [layouts/series/term.html](layouts/series/term.html): series page ordered by part.
- [i18n/](i18n/) and [assets/css/_custom.scss](assets/css/_custom.scss): labels and styles.

`content/posts/python-basics-1` and `python-basics-2` are sample drafts; delete them once real posts exist.

## Deployment

The workflow [.github/workflows/hugo.yaml](.github/workflows/hugo.yaml) builds and deploys on every push to `main`. The Hugo version is declared in `HUGO_VERSION` in the workflow and in the image tag in [compose.yaml](compose.yaml); change both when upgrading.

`static/CNAME` keeps the custom domain across deployments.
