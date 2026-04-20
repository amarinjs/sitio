# amarinjs.github.io

Personal site and technical notebook — [amarinjs.github.io/sitio](https://amarinjs.github.io/sitio/).

Custom Jekyll site, no external theme. Refined-minimal design with editorial serif
display type (Newsreader), Geist sans for UI, JetBrains Mono for code, and a teal
accent. Light and dark modes follow the OS preference.

---

## Running locally

```bash
# one-time, uses the GitHub Pages gemset
bundle install

# serve with live reload
bundle exec jekyll serve

# then open http://localhost:4000/sitio/
```

Note the `/sitio/` path — `baseurl` in `_config.yml` is set to `/sitio` to match
how GitHub Pages serves this repo.

If you just want to build for testing without the full github-pages gem, use
`Gemfile.local` instead (swap filenames: `mv Gemfile Gemfile.ghpages && mv Gemfile.local Gemfile`).
Switch back before pushing to GitHub.

---

## Structure

```
_config.yml          — site-wide settings, author info, socials
_layouts/            — page templates
  default.html         everything wraps in this (head/header/footer)
  home.html            landing page with hero + skills + featured + recent
  post.html            single blog post
  page.html            generic page (about, etc.)
_posts/              — markdown posts, YYYY-MM-DD-slug.md
assets/
  css/main.scss        single stylesheet, all design tokens at the top
  images/              post images
  *.png .ico           favicons
  *.pdf                certifications
index.html           — home page (uses home layout)
about.md             — about page
archive.html         — full post index grouped by year and tag
404.html             — error page
```

---

## How to add a new post

1. Create `_posts/YYYY-MM-DD-my-slug.md`
2. Frontmatter like this:

   ```yaml
   ---
   title: My post title
   tags: [networking, routing]
   excerpt_summary: "One-sentence summary used on the featured cards."
   featured: 1       # optional — 1–4 sets the order of featured cards (lower = first)
   ---
   ```

3. Write markdown below the frontmatter. Code blocks, tables, images all work.
4. Commit and push; GitHub Pages builds and deploys automatically.

### Featuring posts on the home page

Set `featured: N` (where N is 1–4) in frontmatter. The home page shows up to 4
featured posts as cards, sorted ascending by that number — `featured: 1` is
first, `featured: 4` is last. If no posts are featured, the section hides
automatically.

### Tags

Tags are free-form. They link to `/archive/#tag-name`. Keep them lowercase and
hyphenated. Looking at the current archive, the tag list has grown messy — at
some point it's worth consolidating (e.g. `ike-v1`, `ike-v2`, `ike` → `ike`).

---

## Design tokens

All colors and typography live as CSS variables at the top of
`assets/css/main.scss`. To change the accent colour, edit `--accent` (light)
and the `--accent` under `@media (prefers-color-scheme: dark)`.

---

## Known things to improve over time

- Post images load from `github.com/amarinjs/sitio/blob/.../?raw=true` URLs.
  If you ever rename the repo, these all break. Worth moving images into
  `assets/images/` and updating post links to `/assets/images/foo.jpg`.
- The tag list is currently noisy (many tags with just one post). Worth
  consolidating to ~6–8 meaningful tags.
- No search. If the post count grows past ~30, worth adding something like
  [pagefind](https://pagefind.app/).
- No comments. Adding them back (Giscus is the modern Gitalk replacement,
  GitHub-auth, free) is a one-file change if you want them.
