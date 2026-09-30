# hugo-project-landing

The [Hugo](https://gohugo.io) theme behind [cider.mx](https://cider.mx) and
[projectile.mx](https://projectile.mx): a single landing page for an open
source project, with the tool in action up front, a feature showcase, a
spotlight on the current release, setup steps, a FAQ and support links.

It's a Hugo module, so a site imports it rather than copying it:

```toml
# hugo.toml
[module]
  [[module.imports]]
    path = "github.com/bbatsov/hugo-project-landing"
```

Hugo fetches modules with Go, so both Go and Hugo 0.158 or newer are needed.

## What a site provides

**`hugo.toml` params**

| Param | Used for |
|---|---|
| `name` | The project's name, in the nav and section headings |
| `description` | The meta and social description |
| `logo`, `logoStyle` | An SVG under `assets/` for the nav. `logoStyle = "wordmark"` shows it alone; anything else shows it as a mark next to the name. Draw it in `currentColor` so it follows the theme. |
| `docs`, `github`, `repo` | Links to the manual and repository; `repo` (`owner/name`) is where the latest release and star count come from |
| `socialImage` | The Open Graph image, relative to `static/` |
| `sourceRepo`, `copyright` | The footer; `{year}` in `copyright` becomes the current year |
| `fallbackVersion` | Shown when GitHub can't be reached at build time |

**`content/_index.md`**, in the front matter: `tagline`, `lead`, `install`,
`heroMedia`/`heroAlt`, `featuresTitle`, `pillars` (title, blurb and an optional
`icon`) and `steps` (title and body). `startTitle` and `faqTitle` override
the default headings. The body goes under the steps.

**`data/`**: `showcase.yaml` (the feature rows), `features.yaml` (the "and a
lot more" links), `release.yaml` (the spotlight), `faq.yaml` and
`community.yaml` (help, related links, funding and `supportBlurb`).
cider.mx and projectile.mx are working examples of all of these.

**`assets/css/palette.css`**: the site's colors. Every token needs a light
value and a `--dark-` counterpart; the theme's own
[`palette.css`](assets/css/palette.css) is a neutral default that lists them
all.

## Working on the theme

Point a site at a local checkout instead of the published version:

```sh
cd ../cider.mx
HUGO_MODULE_REPLACEMENTS="github.com/bbatsov/hugo-project-landing -> $HOME/projects/hugo-project-landing" hugo server
```

Changes reach the sites through tagged releases: tag a version (`v0.2.0`),
and Dependabot opens a PR bumping `go.mod` in each site, which builds the site
with the new theme before anything is deployed.
