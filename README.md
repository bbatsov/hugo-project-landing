# hugo-project-landing

The [Hugo](https://gohugo.io) theme behind [cider.mx](https://cider.mx) and
[projectile.mx](https://projectile.mx): a single landing page for an Emacs
package that looks like Emacs. There's a header line and a mode line, Org-style
foldable headings, an `M-x` command palette and a few keys, and on wide
screens a second window that shows a screencast or a buffer for whatever
section is being read. It uses the Modus themes' colors and Iosevka type, and
works without JavaScript.

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
| `name` | The project's name |
| `buffer`, `modeName`, `echo` | What the mode lines and the echo area say (`cider.mx`, `Clojure CIDER`, a one-liner) |
| `description`, `socialImage` | The meta description and the Open Graph image |
| `logo`, `logoStyle` | An SVG under `assets/`, drawn in `currentColor`. `logoStyle = "wordmark"` makes it the page's title on its own (give it an `aria-label`); otherwise it's a mark next to the name. |
| `docs`, `github`, `repo` | The manual, the repository, and `owner/name` for the latest release and star count |
| `newsFeed`, `newsTag`, `newsCount` | An Atom feed whose posts with that tag show up in the news, merged with `data/news.yaml` |
| `sourceRepo`, `copyright` | The footer; `{year}` becomes the current year |
| `fallbackVersion` | Shown when GitHub can't be reached at build time |

**`content/_index.md`**: `tagline`, `intro`, `install`, `heroMedia`/`heroAlt`,
`restIntro`, `steps`, `stepsNote` and `outro` in the front matter, and the
project's history as the body. Section headings default to Emacs-ish wording;
override them with `featuresTitle`, `restTitle`, `newsTitle`, `historyTitle`,
`startTitle`, `faqTitle` and `communityTitle`.

**`data/`**: `showcase.yaml` (the feature sections, each with `keys`, a GIF or
screenshot and a caption), `features.yaml` (the "and the rest" links),
`news.yaml` (the news archive), `release.yaml` (`series` and `announcement`,
linked from the opening), `faq.yaml` and `community.yaml` (`help`,
`supportBlurb`, `support` with an optional `recommended`, and optionally
`ecosystem` with `ecosystemIntro`).

**`layouts/_partials/panes/*.html`**: what the second window shows for the
sections without a screencast - `rest`, `news`, `history`, `start`, `faq` and
`community`. Each is optional. Build one from the theme's helpers:

```go-html-template
{{ partial "buffer.html" (dict "name" "*scratch*" "mode" "Lisp Interaction" "body" $html) }}
{{ partial "pane-media.html" (dict "src" "media/shot.webp" "alt" "..." "caption" "...") }}
```

cider.mx shows a REPL, a Magit log and `*Help*`; projectile.mx a dired buffer.

**`assets/css/palette.css`**: the site's colors. The theme's own
[`palette.css`](assets/css/palette.css) lists every token, each with a `--dark-`
counterpart; `--accent` is the brand color and `--region` is used for the
cursor and highlights.

**Other pages** (related projects, a colophon, ...): any markdown file in
`content/` with a `title` and a `description` gets a plain one-column page in
the same frame. Link them from the header line and the footer with menus:

```toml
[[menus.main]]
  name = "related"
  pageRef = "/related"
[[menus.footer]]
  name = "Colophon"
  pageRef = "/colophon"
```

## Working on the theme

Point a site at a local checkout instead of the published version:

```sh
cd ../cider.mx
HUGO_MODULE_REPLACEMENTS="github.com/bbatsov/hugo-project-landing -> $HOME/projects/hugo-project-landing" hugo server
```

Changes reach the sites through tagged releases: tag a version (`v0.3.0`),
and Dependabot opens a PR bumping `go.mod` in each site, which builds the site
with the new theme before anything is deployed.

The bundled Iosevka fonts are subset to Latin and licensed under the SIL Open
Font License (`static/fonts/OFL.txt`).
