# Prompt: Markdown link discovery

**File:** `<link rel="alternate">` in `<head>` + HTTP `Link:` header + `Accept: text/markdown` content negotiation · **Status:** ✅ Standard HTTP · ⚠️ AI tool adoption uneven · **Time to ship:** 30 min

Three different ways to tell AI clients "a markdown version of this page exists, and here's where to find it."

- **`<link rel="alternate">`** in your HTML `<head>`, for crawlers that read HTML.
- **`Link:` HTTP header**, for headless agents that never parse HTML.
- **`Accept: text/markdown` content negotiation**: same URL returns markdown when requested, HTML otherwise. The cleanest approach.

Claude Code, Cursor, and several other coding assistants already send `Accept: text/markdown` as their preferred content type. Content negotiation is the technique most likely to become the long-term standard, because it's just standard HTTP doing what HTTP was designed for. It's the same mechanism that serves JSON vs. HTML from the same URL. It's not cloaking; it's content negotiation declared via `Vary: Accept`.

Prerequisite: you need [`.md` page routes](md-routes.md) shipped first. This file is how you advertise them.

---

## Copy this prompt

```
Help me advertise the markdown versions of my pages to AI clients using three
mechanisms from web standards.

My stack: [DESCRIBE, e.g., "Next.js on Vercel" / "WordPress on cPanel" /
  "Astro on Cloudflare Pages" / "Express on my own VPS" / "Hugo static site"]

My markdown URL pattern: [pick one]
  - Same URL with .md suffix (e.g., /blog/my-post → /blog/my-post.md)
  - Separate API path (e.g., /blog/my-post → /api/markdown/my-post)
  - Content-negotiated on the same URL (no separate path)

Generate three things:

1. The <link rel="alternate" type="text/markdown" href="..."> tag to put in
   <head> of each HTML page, with the correct href pattern for my URLs.

2. The HTTP Link: response header to send on every page, with the correct
   rel="alternate" and type="text/markdown" parameters. Include example
   middleware/route code for my stack.

3. Accept: text/markdown content negotiation: a request handler that returns
   markdown when the client sends Accept: text/markdown, and HTML otherwise.
   The handler must:
   - Return Content-Type: text/markdown; charset=utf-8 for markdown responses
   - Set Vary: Accept on every response. This is critical: without it, CDNs will
     cache the wrong variant for the wrong client.
   - Use a sensible Cache-Control (don't make markdown staler than HTML)
   - Fall through to HTML if Accept is missing or doesn't include text/markdown

Output:
- One code block per mechanism, copy-pasteable into my stack
- Plain-English explanation of where each goes
- A curl command I can use to verify content negotiation works
```

## Heads up

- **`Vary: Accept` is non-negotiable.** If you skip it, your CDN (Vercel, Cloudflare, Fastly) will cache one variant and serve it to everyone. Result: HTML clients get markdown, or vice versa. You'll spend hours debugging.
- **Don't make markdown staler than HTML.** Real example from this repo's author's own site: HTML is `max-age=0, must-revalidate` (Next.js ISR default), markdown is `max-age=3600`. Edits show up on HTML immediately and on markdown an hour later. Match the cache policies, or your AI consumers will see outdated content.
- **AI tooling is inconsistent today.** Some "AI fetch" tools default to `Accept: */*` and run an HTML→markdown conversion server-side instead of asking for markdown directly, even when your site is configured correctly. You can't fix their bug, but if you also expose a direct `.md` route, you cover both cases.
- **Not cloaking.** Search teams sometimes worry serving different content per request looks like cloaking. It's not. Cloaking means lying about content based on User-Agent. Content negotiation responds to a client's explicit `Accept` header, declared with `Vary: Accept`. This is exactly what HTTP was designed for.

## Verify it's working

```bash
# Should return markdown
curl -sI -H "Accept: text/markdown" https://yoursite.com/some-page \
  | grep -iE "content-type|vary"
# Expect: content-type: text/markdown; charset=utf-8
#         vary: Accept

# Should return HTML
curl -sI https://yoursite.com/some-page \
  | grep -iE "content-type|vary"
# Expect: content-type: text/html; charset=utf-8
#         vary: Accept

# Spot-check the <link> tag
curl -s https://yoursite.com/some-page | grep -i 'rel="alternate".*markdown'

# Spot-check the Link: header
curl -sI https://yoursite.com/some-page | grep -i '^link:'
```

If all three signals are present, you're discoverable by every flavor of AI client: those that read HTML, those that don't, and those that just send `Accept: text/markdown`.

## Worked example: Next.js on Vercel

Real-world setup that works (from the author's own site):

- Pretty URL: `https://www.tristandenyer.com/work/some-post` → returns HTML
- Same URL with `Accept: text/markdown` → 200, `content-type: text/markdown`, `vary: Accept`, internal route `/api/markdown/[id]`
- Direct API path: `https://www.tristandenyer.com/api/markdown/some-post` → 200 markdown

Pattern: a Next.js middleware or route handler inspects `Accept`, and if it includes `text/markdown`, rewrites internally to `/api/markdown/[slug]`. The HTML path stays untouched. `Vary: Accept` is set on both responses.

## See also

- [Prompt: `.md` page routes](md-routes.md): ship this first; you need markdown URLs to advertise
- [Prompt: `llms.txt`](llms-txt.md): links into your `.md` routes for AI consumers
- [HTTP RFC 9110 §8.4 (Vary)](https://www.rfc-editor.org/rfc/rfc9110#name-vary)
- [MDN: Content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation)
