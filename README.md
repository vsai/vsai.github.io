# vishalsaidaswani.com

Personal website for Vishalsai Daswani. Built with [Jekyll](https://jekyllrb.com/)
and the [minima](https://github.com/jekyll/minima) theme, hosted on GitHub Pages.

## Run locally

```bash
bundle install        # first time only
bundle exec jekyll serve --livereload
```

Then open <http://localhost:4000>. Pages rebuild automatically on save.

## Structure

- `index.md` — homepage (about blurb)
- `work.md` · `projects.md` · `consultancy.md` · `blog.md` · `random.md` — nav pages
- `_posts/` — blog posts (`YYYY-MM-DD-title.md`)
- `_config.yml` — site config + nav order (`header_pages`)
- `_layouts/home.html` — minimal homepage override (no auto post list)
- `CNAME` — custom domain

## Notes

- Nav order is set by `header_pages` in `_config.yml`.
- The old 2017 site is archived in private repo
  [`vsai/vsai.github.io-archive`](https://github.com/vsai/vsai.github.io-archive).
