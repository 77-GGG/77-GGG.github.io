# Project Memory Outline

## 1) Project Snapshot
- Type: Static blog/site built with Jekyll (GitHub Pages compatible).
- Current site identity:
  - title: `The designs of QI Guo`
  - description: `Artificial Intelligence trends and concepts made easy.`
  - author: `QI Guo`
  - baseurl: `/`
  - url: `https://77-GGG.github.io`
- Primary language in existing content: English (technical/blog articles).

## 2) Core Stack
- Site generator: Jekyll (`Gemfile` + `_config.yml`).
- Theme base: Adam Blog 2.0 (customized).
- Ruby gems:
  - `jekyll`
  - `jekyll-paginate`
  - `jekyll-feed`
  - `jekyll-sitemap`
- Front-end:
  - Liquid templates (`_layouts`, `_includes`, `_pages`)
  - SCSS source under `assets/css/sass/`
  - compiled CSS in `assets/css/main.css`
  - JS with jQuery in `assets/js/main.js`
- Optional local dev pipeline (legacy): Gulp + BrowserSync (`gulpfile.js`, `package.json`).

## 3) Repository Map (What lives where)
- Content:
  - Posts: `_posts/`
  - Standalone pages: `_pages/` (about/tags/archive/tag filter)
- Layout system:
  - `_layouts/default.html` base shell
  - `_layouts/home-page.html` list page container
  - `_layouts/post.html` single post view
  - `_layouts/menu-page.html` content pages
- Reusable partials: `_includes/` (header/footer/search/comments/SEO/sidebar/etc.)
- Static assets:
  - `assets/css/` styles + highlighter themes
  - `assets/js/` scripts
  - `assets/img/` branding, post images, icons
  - `assets/fonts/` local fonts & font-awesome
- Root entry/output helpers:
  - `index.html`, `404.html`, `ipfs-404.html`
  - `sitemap.xml`, `robots.txt`, `search.json`, `all-posts.json`, `posts-by-tag.json`
- CI/CD:
  - `.github/workflows/pages.yml` deploy to GitHub Pages on push to `main`.

## 4) Build, Run, Deploy
- Local (Ruby-native):
  - `bundle install`
  - `bundle exec jekyll serve`
- Local (legacy JS pipeline):
  - `npm install`
  - `npm run dev` (gulp watch + browserSync + sass/img build)
- Production deploy:
  - Automatic via GitHub Actions workflow `pages.yml`.
  - Trigger: push to `main`.

## 5) Content Authoring Conventions
- New post file format:
  - Path: `_posts/YYYY-MM-DD-title.md`
  - Required front matter pattern (observed):
    - `layout: post`
    - `title`, `date`, `img`, `tags`, `category`, `description`
  - Optional/common:
    - `read_time: true`
    - `show_date: true`
    - `author`
- Image references:
  - Cover image via `img: posts/...`
  - In-article images use relative links to `assets/img/posts/...`
- Extra markdown features:
  - `<tweet>...</tweet>` custom tweet block supported by theme.
  - TOC support via page variable `toc` in post layout.
  - MathJax enabled globally (`mathjax: true`).

## 6) Important Config Knobs (`_config.yml`)
- Branding and identity: `title`, `description`, `logo*`, `author*`.
- Dark mode strategy: `night_mode`, `logo-dark`, `highlight_theme`.
- Social links and contact: `email`, `linkedin`, `github`, etc.
- Comments provider:
  - `comments: utteranc` or `disqus`
  - options under `comments_opts`.
- Pagination: `paginate`, `paginate_path`.
- Build behavior:
  - `plugins`, `sass.style`, `compress_html`, `exclude` list.

## 7) UX/Behavior Notes
- Menu/search/scroll-top interactions handled in `assets/js/main.js`.
- Home page cards render from paginator in `index.html`.
- Post page composition includes sidebar, author box, recent posts, newsletter, comments.
- Search UI and tag cloud are integrated via includes.

## 8) High-Value Files to check first during edits
- Site-wide settings: `_config.yml`
- Main page listing: `index.html`
- Single post structure: `_layouts/post.html`
- Global shell/head/scripts: `_layouts/default.html`, `_includes/head.html`, `_includes/javascripts.html`
- Main style source: `assets/css/sass/main.scss` (+ `assets/css/sass/parts/`)
- Compiled style output: `assets/css/main.css`
- Core interaction JS: `assets/js/main.js`
- Deployment workflow: `.github/workflows/pages.yml`

## 9) Risks / Technical Debt (quick memory)
- Gulp toolchain versions are old (`gulp@3` era); may be brittle on modern Node.
- CSS exists in both SCSS sources and compiled `assets/css/main.css`; avoid inconsistent edits.
- Comment config likely incomplete (`comments_opts.repo` empty), so comments may appear disabled.
- Some metadata/wording has minor typos in content/config; maintain consistency if updating.

## 10) Recommended Editing Strategy (for future sessions)
1. Change behavior/layout first in `_layouts`/`_includes`.
2. Change styles in SCSS source; regenerate CSS only if needed by workflow.
3. Keep `_config.yml` as source of truth for identity/global features.
4. For content-only updates, touch only `_posts/` + image assets.
5. Before release-sensitive changes, run local `bundle exec jekyll serve` and verify key pages (`/`, post page, `/about`, `/tags`, `/archive`, `/404.html`).
