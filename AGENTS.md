# Repository Guidelines

## Project Structure & Module Organization
- `_config.yml`: Site settings (title, theme, permalinks, post defaults).
- `_posts/`: Blog posts named `YYYY-MM-DD-slug.md` with YAML front matter.
- `_drafts/`: Unpublished drafts; use `post-template.md` to start new posts.
- `_layouts/`: HTML layouts overriding theme (e.g., `post.html`).
- `index.md`: Home page; lists posts for built‑in themes.
- `tags.md`, `categories.md`: Auto‑generated tag/category indices.

## Build, Test, and Development Commands
- Local preview (Bundler):
  - `gem install bundler jekyll` (once), optionally `bundle add github-pages`.
  - `bundle exec jekyll serve --livereload` → serve at `http://localhost:4000`.
- Local preview (Docker alternative):
  - `docker run --rm -p 4000:4000 -v "$PWD":/srv/jekyll jekyll/jekyll jekyll serve`.
- Production build check: `bundle exec jekyll build -d _site`.
- Optional link check: `bundle exec htmlproofer ./_site --check-html` (if added).

## Coding Style & Naming Conventions
- Posts: `YYYY-MM-DD-descriptive-slug.md` (lowercase, hyphens).
- Front matter: minimal, 2‑space indentation; include `layout`, `title`, `date`, optional `tags`/`categories`.
- Markdown: fenced code blocks with language hints; wrap prose reasonably.
- Liquid: prefer simple filters; keep logic in layouts, not posts.

## Testing Guidelines
- No formal test suite. Validate with `jekyll build` and scan the generated `_site/`.
- If using `htmlproofer`, resolve broken links and HTML errors before pushing.

## Commit & Pull Request Guidelines
- Commit messages: Conventional Commits style where practical (e.g., `feat:`, `fix:`, `docs(scope): ...`).
  - Examples in history: `feat: add clickable tags`, `fix: correct tag links`, `docs(wsl-ai-dev): ...`.
- PRs: clear description, rationale, and screenshots of local build for visual changes; link issues; note any `_config.yml` or theme changes.
- Keep changes focused; separate content updates from layout/config tweaks.

## Security & Configuration Tips
- Use built‑in GitHub Pages themes or approved remote themes to avoid unsupported plugins.
- Do not commit secrets; analytics snippets go in `_includes/head.html` if used.

 
