# brackmann.fr

Source for [www.brackmann.fr](https://www.brackmann.fr), the website of Nathalie Brackmann,
attorney at the Grasse Bar (Sophia Antipolis, France). Built with [Jekyll](https://jekyllrb.com/).

The site is available in three languages:

- French (default) — `/`
- English — `/en/`
- Spanish — `/es/`

## Project structure

```
_config.yml           Site configuration (languages, owner info, etc.)
_data/
  translations.yml     Maps each page to its URL in every language
  i18n.yml              UI strings (nav labels, buttons, footer...) per language
  nav.yml                Order of the main navigation
_layouts/              Page templates (default, home, page-wide)
_includes/              Reusable snippets (header, footer, hero, head)
_sass/, assets/css/    Stylesheets (compiled by Jekyll's built-in Sass support)
assets/js/              A few lines of vanilla JS for the mobile nav menu
assets/images/          Photos and logo
index.md, contact/, …   French pages (no URL prefix)
en/                     English pages
es/                     Spanish pages
```

### Adding or editing a page

Each page's front matter has two keys that tie the multilingual setup together:

- `lang`: the page's language (`fr`, `en`, or `es`)
- `ref`: a shared key used to find the equivalent page in the other languages
  (must match a key in `_data/translations.yml`)

To edit existing content, just edit the relevant Markdown file — there's no
build step for text changes. To add a brand-new page in all three languages,
duplicate one of the existing pages, add an entry to `_data/translations.yml`
(and `_data/i18n.yml`/`_data/nav.yml` if it should appear in the main menu).

## Testing the site locally

You'll need [Ruby](https://www.ruby-lang.org/) 3.1 or newer.

**1. Install dependencies** (once, and again whenever `Gemfile` changes):

```bash
bundle install
```

**2. Start the local server with live rebuild:**

```bash
bundle exec jekyll serve --livereload
```

Open [http://localhost:4000](http://localhost:4000) in your browser. The page
reloads automatically whenever you save a file.

**3. Or, build the static site without serving it** (useful to double-check
the production build before pushing):

```bash
bundle exec jekyll build
```

The generated site is written to `_site/`.

### Troubleshooting

- `bundle: command not found` — install the Bundler gem: `gem install bundler`.
- On Ruby 3.0+, `webrick` is required for `jekyll serve` to work; it's already
  listed in the `Gemfile`, so `bundle install` takes care of it.
- If a Ruby version manager (rbenv, RVM, asdf) is installed, make sure it's
  picking up a Ruby version that matches the one described above.

## Deployment

Pushing to the `gh-pages` branch (which happens automatically when a pull
request is merged into it) triggers the **Build and deploy site** GitHub
Actions workflow (`.github/workflows/deploy.yml`), which builds the Jekyll
site and publishes it to GitHub Pages. Every pull request also runs a
**Build check** workflow that builds the site to catch errors before merge.

> **One-time setup:** in the repository's **Settings → Pages**, the source
> must be set to **"GitHub Actions"** (not "Deploy from a branch") for the
> deploy workflow to be allowed to publish the site.

The custom domain is configured via the `CNAME` file at the repository root
and the site's DNS records (unchanged by this redesign).

## Framework version

The site runs on Jekyll 4.3, the latest stable release, with only two small,
actively maintained plugins (`jekyll-sitemap`, `jekyll-seo-tag`). The
previous version of this site ran on Jekyll ~> 3.0 (released in 2015) with
the unmaintained `octopress` gem; both have been dropped.
