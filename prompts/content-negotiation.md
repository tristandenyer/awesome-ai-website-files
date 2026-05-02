# Prompt: `Accept: text/markdown` content negotiation

**File:** Server / middleware behavior on every page URL · **Status:** ✅ Standard HTTP · ⚠️ AI tool adoption uneven · **Time to ship:** 30 min to 2 hr depending on stack

Same URL, two representations. When a client sends `Accept: text/markdown`, your server returns the clean markdown variant. When the same URL is requested with the default `Accept: text/html`, it returns the HTML page. The variant is selected by the request's `Accept` header, declared back to caches via `Vary: Accept`.

Claude Code, Cursor, and several other coding assistants already send `Accept: text/markdown` as their preferred content type. Content negotiation is the technique most likely to become the long-term standard, because it's just standard HTTP doing what HTTP was designed for. It's the same mechanism that serves JSON vs. HTML from the same URL.

Prerequisite: you need clean markdown content available for each page. Either ship [`.md` page routes](md-routes.md) first and have content negotiation rewrite to them internally, or generate markdown on-the-fly in the handler.

---

## Copy this prompt

```
Implement Accept: text/markdown content negotiation for my website. When a
client requests one of my page URLs with Accept: text/markdown, return clean
markdown. Otherwise, return the HTML page. The URL stays the same in both
cases.

My stack: [DESCRIBE, e.g., "Next.js on Vercel" / "WordPress on cPanel" /
  "Astro on Cloudflare Pages" / "Express on my own VPS" / "Hugo static site" /
  "Cloudflare Worker in front of origin"]

Where my markdown content comes from: [pick one]
  - Existing .md routes (e.g., /blog/post.md). Rewrite internally based on Accept.
  - Existing API path (e.g., /api/markdown/[slug]). Rewrite internally based on Accept.
  - Generated on-the-fly from the page's source content
  - Not yet implemented; tell me what to do first

Generate:

1. The request handler / middleware code. It must:
   - Inspect the request's Accept header
   - If Accept includes text/markdown (with a higher q-value than text/html
     where applicable), return the markdown variant
   - Otherwise return the HTML variant
   - Set Content-Type: text/markdown; charset=utf-8 on markdown responses
   - Set Vary: Accept on EVERY response (HTML and markdown both). This is
     critical: without it, CDNs will cache the wrong variant for the wrong
     client.
   - Use a Cache-Control that matches the HTML variant's freshness. Don't
     let markdown go stale while HTML is fresh, or vice versa.
   - Fall through cleanly to HTML when Accept is missing, malformed, or
     doesn't include text/markdown

2. CDN / edge configuration notes for my stack:
   - Where to set Vary: Accept (origin? edge? both?)
   - Any platform-specific gotchas with Accept-based caching (Vercel,
     Cloudflare, Fastly, Akamai all behave slightly differently)

3. A test plan with curl commands that verify:
   - text/markdown is returned with the right header
   - Default request returns HTML
   - Vary: Accept is present on both
   - The markdown content matches what would be at the .md route (if I have one)

Output: code blocks per piece, plain-English explanation of each, and one
"things I should test before shipping" checklist.
```

## Heads up

- **`Vary: Accept` is non-negotiable.** Without it, your CDN (Vercel, Cloudflare, Fastly) will cache one variant and serve it to everyone. Result: HTML clients get markdown, or vice versa. You'll spend hours debugging.
- **Match cache freshness.** Real example from this repo's author's own site: HTML is `max-age=0, must-revalidate` (Next.js ISR default), while the markdown variant was set to `max-age=3600`. Edits showed up on HTML immediately and on markdown an hour later. AI consumers saw stale content. Match the cache policies.
- **Quality-value (`q=`) matters at the margin.** Some clients send `Accept: text/html, text/markdown;q=0.9`, which means "I prefer HTML, but markdown is acceptable." Don't blindly serve markdown to anything that mentions it. Parse properly, or use a library.
- **AI tooling is inconsistent today.** Some "AI fetch" tools default to `Accept: */*` and run an HTML→markdown conversion server-side instead of asking for markdown directly, even when your site is configured correctly. You can't fix their bug, but if you also expose direct [`.md` page routes](md-routes.md), you cover both cases.
- **Not cloaking.** Search teams sometimes worry serving different content per request looks like cloaking. It's not. Cloaking means lying about content based on User-Agent. Content negotiation responds to a client's explicit `Accept` header, declared with `Vary: Accept`. This is exactly what HTTP was designed for: it's the same mechanism REST APIs use to return JSON vs. XML.
- **Edge vs. origin.** If you're behind a CDN, deciding whether to do the negotiation at the edge or at the origin affects cache hit rates significantly. Edge negotiation with proper `Vary: Accept` is usually fastest. Origin-only is simpler but slower.

## Verify it's working

```bash
# Markdown request returns markdown with Vary: Accept
curl -sI -H "Accept: text/markdown" https://yoursite.com/some-page \
  | grep -iE "content-type|vary"
# Expect: content-type: text/markdown; charset=utf-8
#         vary: Accept

# Default request returns HTML with Vary: Accept
curl -sI https://yoursite.com/some-page \
  | grep -iE "content-type|vary"
# Expect: content-type: text/html; charset=utf-8
#         vary: Accept

# Body actually differs (markdown should not contain <html> or <nav>)
diff \
  <(curl -s -H "Accept: text/markdown" https://yoursite.com/some-page | head -20) \
  <(curl -s https://yoursite.com/some-page | head -20) \
  | head -10
```

If the bodies are identical, your handler isn't actually switching on `Accept`. If `Vary: Accept` is missing on either response, your CDN will eventually break things.

## Worked example: Next.js on Vercel

Real-world setup that works (from the author's own site):

- Pretty URL: `https://www.tristandenyer.com/work/some-post` returns HTML.
- Same URL with `Accept: text/markdown` returns 200, `content-type: text/markdown; charset=utf-8`, `vary: Accept`. Internally Next.js rewrites to `/api/markdown/[id]`.
- Direct API path: `https://www.tristandenyer.com/api/markdown/some-post` returns 200 markdown.

Pattern: a Next.js middleware inspects `Accept`, and if it includes `text/markdown`, rewrites internally to `/api/markdown/[slug]`. The HTML path stays untouched. `Vary: Accept` is set on both responses. The HTML and markdown share the same source content so edits propagate consistently.

## See also

- [Prompt: `.md` page routes](md-routes.md): the explicit URL alternative; ship this first or alongside content negotiation
- [Prompt: Markdown link discovery](markdown-discovery.md): `<link rel="alternate">` tag and HTTP `Link:` header to advertise the markdown variant
- [HTTP RFC 9110 §8.4 (Vary)](https://www.rfc-editor.org/rfc/rfc9110#name-vary)
- [HTTP RFC 9110 §12 (Content Negotiation)](https://www.rfc-editor.org/rfc/rfc9110#name-content-negotiation)
- [MDN: Content negotiation](https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation)
