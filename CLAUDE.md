# CLAUDE.md — personal website

Personal site for Vishalsai Daswani. **Jekyll + minima 2.5.2**, served by **GitHub Pages**
from `master`, live at **https://vishalsaidaswani.com**.

## Local development

```
bundle exec jekyll serve --livereload
```

## Deploy

Push to `master`. GitHub Pages rebuilds and redeploys automatically (native Jekyll build).

## Gotchas (not obvious from the code)

- **Watch for CNAME auto-commits when pushing.** Poking the GitHub Pages custom-domain API
  makes GitHub auto-commit "Create CNAME" / "Delete CNAME" directly to `master`. If that has
  happened the remote is ahead — `git pull --rebase` before pushing, or the push is rejected.

- **rbenv `CXX="false"` toolchain bug (arm64 macOS).** rbenv Ruby 3.4.x ships
  `CONFIG["CXX"] = "false"` in its `rbconfig.rb`, which silently breaks native C++ gems (e.g.
  `eventmachine`, pulled in by jekyll livereload) — the build fails with no compiler error.
  Fix: set that value to `clang++` in the Ruby version's `rbconfig.rb`. If a newly installed
  Ruby fails to build C++ gems, check `ruby -e 'puts RbConfig::CONFIG["CXX"]'` first.

## Conventions

- **Draft posts** stay local: listed in `.gitignore` so they preview locally but never
  commit. Publish by removing their line from `.gitignore`, then committing.
- **Blog diagrams** are hand-authored `.excalidraw` JSON in `assets/diagrams/`, exported to
  PNG. The `.excalidraw` sources are committed but excluded from the Jekyll build
  (`_config.yml`); only the PNGs ship.
- **Old site versions** are archived in the private repo `vsai/vsai.github.io-archive`.
