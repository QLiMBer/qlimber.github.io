# QLiMBer Blog – Maintenance Guide

## Live Site

- Website: https://qlimber.github.io

A concise reference for how this site works, how to change themes and layout, and the safest ways to customize.

## How the site works (GitHub Pages + Jekyll)

- **Engine**: Jekyll (static site generator). GitHub Pages builds and hosts it automatically on each push to `main`.
- **Content model**: Markdown posts under `_posts/` named `YYYY-MM-DD-slug.md`.
- **Templates**: Provided by the selected theme. Jekyll merges your Markdown with theme layouts to generate HTML.
- **Gem**: A Ruby package. Many themes (e.g., `minima`) are shipped as gems. You don’t edit the gem; you override parts of it in your repo when needed.

## Repo file map (what controls what)

- `_config.yml`: Global settings (site title/description, selected theme, permalink style, defaults).
- `index.md`: The homepage. The layout you choose here determines how the front page renders.
- `_posts/`: Blog posts. Each file can define `layout`, `title`, `date`, etc. in its front matter.
- Optional override points (only create if you need to customize):
  - `_layouts/*.html`: Page skeletons. Create to override a theme’s layout.
  - `_includes/*.html`: Small reusable fragments like `header.html`, `footer.html`, analytics helpers.
  - `assets/main.scss` or `_sass/*.scss`: CSS overrides.
- Additional project files:
  - `_layouts/default.html`: Custom wrapper that injects nav links and analytics consent.
  - `_layouts/page.html`: Simple page layout that displays the title.
  - `_layouts/post.html`: Post layout with title header plus tag/category footer.
  - `_includes/head-custom.html`: Inline CSS for the cookie consent banner.
  - `_includes/analytics-consent.html`: Banner + GA initialisation logic (honours consent & IP anonymisation).
  - `privacy-policy.md`: Published policy linked from the header.

## Local development workflow

1. **Install Ruby (once per machine)**  
   Using `rbenv` keeps versions isolated:
   ```bash
   rbenv install 3.3.4
   rbenv global 3.3.4
   gem install bundler
   ```

2. **Install project gems**  
   ```bash
   cd ~/projects/qlimber.github.io
   bundle config set path 'vendor/bundle'
   bundle install
   ```

3. **Run a live preview**  
   ```bash
   bundle exec jekyll serve --livereload
   ```
   Open `http://127.0.0.1:4000/`. Test the cookie banner: consenting loads GA (with `anonymize_ip`), declining keeps analytics disabled. Stop the server with `Ctrl+C`.

4. **Production build check**  
   ```bash
   bundle exec jekyll build
   ```
   Inspect `_site/` if you need to review the generated HTML.

5. **Optional QA**  
   - `bundle exec htmlproofer ./_site --check-html` (if you install `htmlproofer`)  
   - Manual smoketest of GA real-time dashboard after deploying and accepting the banner.

## Built-in themes vs remote themes (practical notes)

- **Built-in themes** (auto-available on GitHub Pages): set `theme: theme-name` in `_config.yml`.
  - Blog-suitable, dark-friendly options you used:
    - `minima` (has blog layouts; can enable dark skin)
    - `jekyll-theme-midnight` (dark; project-oriented)
    - `jekyll-theme-hacker` (dark/green; project-oriented)
- **Remote themes** (fetched from a GitHub repo): set `remote_theme:` and enable the plugin.
  - Example: `remote_theme: pages-themes/midnight@v0.2.0` plus plugin `jekyll-remote-theme`.

## Theme switching cheatsheet (your three favorites)

- Minima (dark blog):

  ```yaml
  # _config.yml
  title: QLiMBer Blog
  description: My notes on various topics
  theme: minima
  minima:
    skin: dark
  ```
  
  ```yaml
  # index.md (front matter only)
  ---
  layout: home
  ---
  ```

- Built-in Midnight (dark):

  ```yaml
  # _config.yml
  title: QLiMBer Blog
  description: My notes on various topics
  theme: jekyll-theme-midnight
  ```
  
  ```markdown
  # index.md (use default + posts list)
  ---
  layout: default
  title: Home
  ---
  <h1>Posts</h1>
  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
        <h2><a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      </li>
    {% endfor %}
  </ul>
  ```

- Built-in Hacker (dark/green):

  ```yaml
  # _config.yml
  title: QLiMBer Blog
  description: My notes on various topics
  theme: jekyll-theme-hacker
  ```
  
  ```markdown
  # index.md same as Midnight (use default + posts list)
  ---
  layout: default
  title: Home
  ---
  <h1>Posts</h1>
  <ul class="post-list">
    {% for post in site.posts %}
      <li>
        <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
        <h2><a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
      </li>
    {% endfor %}
  </ul>
  ```

Notes:

- `minima` provides a `home` layout that auto-lists posts, so `index.md` needs no HTML.
- `midnight` and `hacker` do not provide a blog home layout, so the simple posts list is added to `index.md`.
- Alternative to adding HTML in `index.md`: create your own `_layouts/home.html` that lists posts and set `index.md` back to `layout: home`.

## Implemented features (current state)

- **Post defaults**: All posts automatically use `layout: post` via `_config.yml` defaults.

  ```yaml
  # _config.yml
  defaults:
    - scope: { path: "", type: posts }
      values: { layout: post }
  ```

- **Permalinks**: Human-friendly URL structure.

  ```yaml
  # _config.yml
  permalink: /:year/:month/:day/:title/
  ```

- **Tags & categories**: Clickable tags/categories on each post plus index pages at `/tags/` and `/categories/`.
  - Local override layout: `_layouts/post.html` (renders linked tags/categories)
  - Index pages: `tags.md`, `categories.md`
- **Post template**: `_drafts/post-template.md` to quickly start new posts with consistent front matter.

## Safe overrides (how to customize without touching the theme gem)

- Post layout that links tags and categories (current implementation):

  ```html
  <!-- _layouts/post.html -->
  ---
  layout: default
  ---
  <article class="post">
    <header class="post-header">
      <h1>{{ page.title }}</h1>
    </header>

    {{ content }}

    {% assign has_tags = page.tags | default: empty %}
    {% assign has_categories = page.categories | default: empty %}
    {% if has_tags != empty or has_categories != empty %}
      <hr>
      <footer class="post-meta">
        {% if has_tags != empty %}
          <p><strong>Tags:</strong>
          {% for tag in page.tags %}
            {% assign tag_slug = tag | slugify %}
            <a href="{{ '/tags/#' | append: tag_slug | relative_url }}">{{ tag }}</a>{% unless forloop.last %} · {% endunless %}
          {% endfor %}
          </p>
        {% endif %}
        {% if has_categories != empty %}
          <p><strong>Categories:</strong>
          {% for category in page.categories %}
            {% assign category_slug = category | slugify %}
            <a href="{{ '/categories/#' | append: category_slug | relative_url }}">{{ category }}</a>{% unless forloop.last %} · {% endunless %}
          {% endfor %}
          </p>
        {% endif %}
      </footer>
    {% endif %}
  </article>
  ```

- Style tweaks:

  ```scss
  ---
  ---
  /* assets/main.scss */
  @import "minima"; /* or omit for built-in themes that don’t expose a Sass entry */
  .site-title { font-weight: 700; }
  ```

## Ideas / next steps

- **Optional navigation in header**: Add quick links (e.g., `About`, `Projects`, `Tags`, `Categories`). `jekyll-theme-hacker` may not render `header_pages` by default; add your own `_includes/header.html` or `_includes/nav.html` to display links.
  - Potential config if you adopt a theme that supports it:

    ```yaml
    header_pages:
      - about.md
      - projects.md
      - tags.md
      - categories.md
    ```

- **Analytics**:
  - GA4 is wired through `_includes/head-custom.html` (loads the standard gtag snippet with consent defaulted to denied for Search Console compatibility) and `_includes/analytics-consent.html` (banner + consent handling).
  - Consent banner stores state in `localStorage` and only enables GA after acceptance; declining keeps analytics disabled and updates consent back to denied.
  - Privacy Policy lives at `/privacy-policy/`; update it whenever tracking scope changes.
  - GitHub Insights still provides basic repo traffic metrics if you need a double-check.

## Workflow: add a new post (concise)

1. Copy the template: `_drafts/post-template.md` → `_posts/YYYY-MM-DD-your-title.md`.
2. Update front matter: `title`, `date` (match filename for clarity), `categories`, `tags`.
3. Write content in Markdown below the front matter.
4. Push to `main` — GitHub Pages will rebuild automatically.
Notes:

- Posts must include a front matter block (`---` at top) to be processed.
- `layout: post` is auto-applied by defaults; you can override per post if needed.

## Local preview (fast feedback loop)

- Follow the steps in [Local development workflow](#local-development-workflow). That uses Bundler with the exact GitHub Pages gem set.
- On Windows, WSL2 is recommended. Run the commands from within your WSL shell; LiveReload still serves at `http://127.0.0.1:4000`.

## Troubleshooting quick answers

- Empty homepage after switching theme:
  - Cause: the theme lacks a `home` layout.
  - Fix: use `layout: default` with the posts list HTML in `index.md`, or create an override `_layouts/home.html` that lists posts and set `index.md` to `layout: home`.
- Duplicate site title appearing in multiple places:
  - Header/footer are pulling `site.title` and `site.description` from `_config.yml`. Override `_includes/footer.html` to change what shows.

## Theme links

- Minima: `https://github.com/jekyll/minima`
- Midnight (built-in and remote): `https://github.com/pages-themes/midnight`
- Hacker: `https://github.com/pages-themes/hacker`
