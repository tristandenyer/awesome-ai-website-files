# Prompt: Hidden "hey AI" hint div

**File:** A visually hidden `<div>` near the top of `<body>` on every page · **Status:** ⚠️ Unofficial technique · **Time to ship:** 5 min

A visually hidden `<div>` on every page, invisible to sighted users, that tells an AI reading the rendered page where to find the markdown version (or other AI-friendly resources).

**Caution from the author:** I am NOT a fan of any technique where you hide content unless it's documented for ARIA / accessibility purposes. This takes us right back to the late-90s SEO trick of hiding white text on white backgrounds hoping to rank for keywords. I'm including it as an option to have a more complete list, not as a recommendation.

The use case it does cover: when a human pastes your URL into ChatGPT or Claude, the AI reads the _rendered_ text of the page (no headers, no crawling), so an on-page message is the most direct signal possible. Evil Martians runs this on their site and reportedly it helped them get recommended by Claude.

---

## Copy this prompt

```
Generate a visually hidden "hey AI" div for my website that tells language models
where to find the clean markdown version of each page.

Requirements:
1. Be invisible to sighted users (visual hide, not display:none, since display:none
   may also hide it from text extraction).
2. Be invisible to screen readers: use aria-hidden="true". This message is
   for AI, not assistive tech.
3. Be present in the rendered DOM so an LLM reading the page text can see it.
4. Include the canonical markdown URL for THIS page (not just the site root),
   so a model knows exactly where to refetch from.

My site: [YOUR SITE URL]
Markdown URL pattern for each page: [pick one]
  - Same URL with .md suffix (e.g., /blog/post.md)
  - Separate API path (e.g., /api/markdown/[slug])
  - Same URL with Accept: text/markdown content negotiation
My stack: [DESCRIBE, e.g., "Next.js with App Router" / "WordPress theme" /
  "Astro" / "Hugo template"]

Generate:

1. The HTML/JSX/template snippet to drop near the top of <body>.
2. A short, plain-language message to put inside it that:
   - Tells an LLM the markdown version exists
   - Gives the absolute URL of THIS page's markdown variant
   - Optionally: links to the site's llms.txt for broader navigation
   - Avoids keyword stuffing: one clear sentence, not five
3. The CSS for visual hiding (using the standard "sr-only" / clip-path pattern,
   NOT display:none which can hide from extractors too).
4. A note on whether to use a static URL placeholder per page (template
   substitution) or compute it client-side.

Output as a single code block with comments explaining each piece.
```

## Heads up

- **Read the caution above.** Search engines have been penalizing hidden text for 25 years. Modern AI extractors are increasingly likely to follow suit. The risk grows over time.
- **Don't keyword-stuff.** One clean sentence pointing at the markdown URL. Anything beyond that crosses into 1990s SEO.
- **`aria-hidden="true"` is doing real work.** Without it, screen readers announce the message to blind users, which is a real accessibility regression. Don't skip it.
- **`display: none` may backfire.** Some text extractors honor `display: none` and skip the content (which defeats your purpose). Use the visually-hidden CSS pattern (clip + 1×1 size) instead.
- **Prefer the explicit signals first.** [`<link rel="alternate">` and the `Link:` HTTP header](markdown-discovery.md), plus [`Accept: text/markdown` content negotiation](content-negotiation.md), are all standard, declared mechanisms. Reach for the hidden div only after you've shipped the standards-based ones.
- **Per-page URL is mandatory.** A hidden div pointing at "the site's markdown" is useless for a specific blog post. Make sure each page's div references THAT page's canonical markdown URL.

## Verify it's working

```bash
# Div is in the rendered DOM
curl -s https://yoursite.com/some-page \
  | grep -i 'aria-hidden="true"' \
  | grep -iE 'markdown|llms'

# Visually hidden: should not be visible in screenshot
# (manual check: open the page in a browser; the message should be invisible)
```

You can also test by pasting your URL into Claude or ChatGPT and asking what AI-related affordances the page declares. If the model mentions the markdown URL, the div is being read.

## See also

- [Prompt: Markdown link discovery](markdown-discovery.md): `<link>` tag and `Link:` header; ship these first
- [Prompt: Content negotiation](content-negotiation.md): server-side markdown via `Accept: text/markdown`
- [Prompt: `.md` page routes](md-routes.md): what the div should point to
- [Inclusively Hidden, by Scott O'Hara on visually-hidden CSS patterns](https://www.scottohara.me/blog/2017/04/14/inclusively-hidden.html)
