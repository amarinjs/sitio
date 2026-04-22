---
title: "Jekyll feature reference — a living cheat-sheet"
tags: [jekyll, reference, markdown, kramdown]
excerpt_summary: "One post exercising every Markdown, kramdown, Rouge, and Liquid feature my site currently supports. Kept here so I can see what works, what doesn't, and what I'd need to enable later."
---

Notes to future me. Each section is a minimal working example plus a one-liner on when I'd actually reach for it. If something renders wrong on the live site, it goes on the checklist at the bottom.

## Headings

Use `h2` for top-level sections of a post — `h1` already comes from the frontmatter `title` via `post.html`. `h3` for subsections, `h4` for the occasional nested bit. Deeper than that and the post is trying to be a book.

### Level 3 — subsection
#### Level 4 — rarely

## Text formatting

For emphasis and in-line references.

A paragraph with **bold**, *italic*, ***bold italic***, ~~strikethrough~~, and `inline code`. Use bold for things I actually want to call out, italic for titles or the first mention of a term, strikethrough only in reference notes where I want to show what changed.

## Lists

Ordered for steps that have to happen in that order. Unordered for bullet points where order doesn't matter. Nested for a hierarchy I don't want to split into sections.

1. First step
2. Second step
3. Third step

- Item
- Another item
- Third item
  - Nested child
  - Another nested child
    - Deeper still

### Task list

For tracking what's done inside a reference post.

- [x] Finish the draft
- [x] Add example code
- [ ] Review once deployed
- [ ] Add a screenshot if something rendered wrong

## Horizontal rule

Breaks a post into real sections when headings would be overkill.

---

## Blockquotes

For quoting docs or someone else's words.

> A quoted paragraph. Use this when the exact wording matters.

> An outer quote.
>
>> A nested quote inside it — rare, but occasionally useful for reply-style notes.

## Footnote

For sidebar facts I don't want to break the flow of a sentence[^1].

[^1]: This is the footnote definition. Kramdown collects all `[^n]` definitions and renders them at the bottom of the post with back-links.

## Abbreviations — not supported

Kramdown's abbreviation syntax (`*[HTML]: Hyper Text Markup Language`, which would make `<abbr>` tooltips) is a kramdown extension that GitHub Pages' GFM parser (`kramdown-parser-gfm`) does not process. Broke the first build of this post. Leaving the syntax out and noting it here. If I ever need it, the workaround is inline HTML: `<abbr title="Hyper Text Markup Language">HTML</abbr>`.

## Definition list — not supported

Same story: kramdown's `term\n:   definition` definition-list syntax isn't in the GFM parser. For term/definition pairs I use a two-column table or inline HTML `<dl>` instead.

| Term     | Definition                                                                          |
| :------- | :---------------------------------------------------------------------------------- |
| Kramdown | Markdown parser Jekyll uses.                                                         |
| Rouge    | Ruby syntax highlighter. Runs at build time, emits plain HTML with CSS classes.      |
| Liquid   | Jekyll's template language. Runs during `jekyll build`.                              |

## Links

### Inline link

[kramdown syntax reference](https://kramdown.gettalong.org/syntax.html) — when I want the URL right next to the anchor.

### Reference-style link

Reference-style keeps paragraphs readable when several links reuse the same URL. I use this more in longer posts.

See the [Jekyll docs][jekyll] or the [kramdown docs][km] for the canonical detail.

[jekyll]: https://jekyllrb.com/docs/
[km]: https://kramdown.gettalong.org/syntax.html

### Automatic link

Bare URLs in angle brackets render as a clickable link without any anchor text: <https://jekyllrb.com/>.

## Images

### Plain Markdown image

For diagrams, screenshots, or anything I want inline without a caption. The path goes through `{{ site.baseurl }}` so it resolves both locally and on GitHub Pages.

![Plain placeholder image — blue background]({{ site.baseurl }}/assets/images/posts/jekyll-feature-reference/placeholder-plain.svg)

### Captioned figure

Kramdown with `input: GFM` passes raw HTML through, so `<figure>` + `<figcaption>` works. Use this when the caption is actually useful context and not just the same thing the heading already says.

<figure>
  <img src="{{ site.baseurl }}/assets/images/posts/jekyll-feature-reference/placeholder-captioned.svg" alt="Captioned placeholder image — sand background">
  <figcaption><em>Figure 1. A captioned placeholder. Use figures when the caption adds information the alt text and surrounding prose don't.</em></figcaption>
</figure>

## Code

### Fenced block with no language

Useful for plain terminal output or raw text where syntax highlighting would be misleading.

```
show archive config differences
!No changes were found
```

### Bash

```bash
#!/usr/bin/env bash
set -euo pipefail

for host in router-01 router-02 switch-03; do
  ssh "$host" 'show version | include uptime'
done
```

### Python

```python
from ipaddress import ip_network

def summarize(prefixes):
    nets = [ip_network(p) for p in prefixes]
    return list(ip_network.collapse_addresses(nets))

print(summarize(["10.0.0.0/24", "10.0.1.0/24"]))
```

### YAML

```yaml
plugins:
  - jekyll-feed
  - jekyll-paginate
  - jekyll-sitemap
  - jekyll-seo-tag
```

### JSON

```json
{
  "device": "rtr-core-01",
  "drift": true,
  "lines": ["+interface Gi0/0", "+shutdown"]
}
```

### HTML

```html
<figure>
  <img src="/assets/images/posts/jekyll-feature-reference/placeholder-plain.svg" alt="plain">
  <figcaption>Figure caption.</figcaption>
</figure>
```

### Cisco-style (text fence)

No native `cisco` lexer exists in Rouge, and existing posts on this site use plain text fences for IOS config. Matches the convention.

```text
interface GigabitEthernet0/0
 description UPLINK
 no ip address
 shutdown
!
line vty 0 4
 transport input ssh
 login authentication default
!
```

### Line highlighting

Rouge supports per-line highlighting via the Liquid `{% highlight %}` tag with the `mark_lines` option, but not via fenced code blocks on vanilla GitHub Pages. Note: feature is limited on this site — leaving it off by default and using comments (`# <-- note`) inside the code when I need to call out a line.

### Inline code and escaping backticks

A sentence with `inline code` for command names and flag values like `--no-verify`. To show a backtick inside inline code, wrap the whole thing in double backticks: `` `write mem` `` renders with its backticks intact.

## Table

Pipe tables with mixed alignment. Left for labels, right for numbers, centered for short status fields.

| Feature              |   Status    | Notes about when to use it |
| :------------------- | :---------: | -------------------------: |
| Footnotes            |   enabled   |         sidebar-style facts |
| Abbreviations        | **not on**  |       kramdown-only, GFM off |
| Definition lists     | **not on**  |       kramdown-only, GFM off |
| Math (MathJax/KaTeX) | **not on**  |      would need layout edit |
| Diagrams (Mermaid)   | **not on**  |      would need layout edit |

## Math and diagrams — not currently enabled

**MathJax / KaTeX and Mermaid are not enabled on this site.** There's no loader in `_layouts/default.html`, no `mathjax: true` branch, and the `_config.yml` doesn't pull either plugin. Leaving the syntax out of this post so nothing renders as raw `$$…$$` or broken mermaid blocks.

To turn on MathJax later, the minimal change is: add a `<script>` tag to `_layouts/default.html` (or gate it on `{% if page.math %}`) pointing at a MathJax CDN, then set `math: true` in a post's frontmatter. For Mermaid the same shape applies — add the mermaid JS loader and convert `<pre class="mermaid">` blocks to rendered diagrams on `DOMContentLoaded`.

## Liquid and Jekyll-specific

### Showing Liquid syntax literally

To display Liquid tags on the page without Jekyll executing them, wrap them in `{% raw %}` / `{% endraw %}`. This is how I'd document a template in a future post.

{% raw %}
```liquid
{% for post in site.posts limit: 5 %}
  {{ post.title }} — {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}
```
{% endraw %}

### Liquid resolving at build time

These four values come from Liquid, executed during `jekyll build`. If the page renders them correctly, the template engine is wired up.

- `page.title`  →  **{{ page.title }}**
- `page.date` formatted  →  **{{ page.date | date: "%B %-d, %Y" }}**
- `site.title`  →  **{{ site.title }}**
- `site.author.name`  →  **{{ site.author.name }}**

### Rouge highlight via Liquid tag

Alternative to fenced blocks. Useful when I want options like `linenos` on a specific snippet without changing site config.

{% highlight ruby %}
# Ruby example — unlikely to appear on a network blog,
# but it exercises Rouge via the Liquid tag path.
posts = Dir.glob("_posts/*.md").sort
posts.last(3).each { |p| puts p }
{% endhighlight %}

### `{% include %}` — skipped

No `_includes/` directory exists in the repo at the moment, so there's nothing to include. Skipping rather than inventing one.

## What didn't render correctly?

After the post is deployed, I go through this list on the live page and tick what worked. Anything unchecked is a real rendering bug to fix.

- [ ] Headings h2, h3, h4 all render with distinct sizes
- [ ] Bold / italic / bold italic / strikethrough / inline code all visually distinct
- [ ] Ordered, unordered, and 3-level nested lists render correctly
- [ ] Task list checkboxes appear (checked vs unchecked)
- [ ] Horizontal rule shows a line
- [ ] Blockquote + nested blockquote render with visible nesting
- [ ] Footnote link jumps to the definition and back
- [ ] Inline, reference-style, and auto-link all work and underline on hover
- [ ] Plain image loads and sizes correctly
- [ ] Captioned figure shows the caption below the image
- [ ] Every code block has a distinct syntax-highlighting style
- [ ] Text fence (Cisco config) renders as monospace without syntax errors
- [ ] Inline code with escaped backticks shows the backticks
- [ ] Table renders with correct column alignment (left / center / right)
- [ ] Liquid raw block shows the tag syntax literally
- [ ] Liquid values (`page.title`, `page.date`, `site.title`, `site.author.name`) resolve to real text
- [ ] `{% highlight ruby %}` block highlights like a fenced block would
