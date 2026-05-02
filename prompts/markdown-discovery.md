# Prompt: Markdown link discovery (`<link>` tag + `Link:` header)

**File:** `<link rel="alternate">` in `<head>` + HTTP `Link:` response header · **Status:** ✅ Standard HTML and HTTP · ⚠️ AI tool adoption uneven · **Time to ship:** 15 min

Two declared, standards-based ways to advertise that a markdown version of a page exists:

- **`<link rel="alternate" type="text/markdown" href="...">`** in your HTML `<head>`, for crawlers and tools that read HTML.
- **`Link:` HTTP response header**, for headless agents that never parse HTML.

Both point at the same target: a clean markdown URL for the page (typically a [`.md` route](md-routes.md) like `/blog/post.md`, or an API path like `/api/markdown/post`).

For the third, server-side mechanism (same URL serves markdown when the client sends `Accept: text/markdown`), see the separate [content-negotiation](content-negotiation.md) prompt. The two approaches are complementary: ship both for full coverage.

Prerequisite: you need a markdown URL to point at. Either ship [`.md` page routes](md-routes.md) first, or have an API path that returns markdown.

---

## Copy this prompt

```
Help me advertise the markdown versions of my pages to AI clients using two
standards-based discovery mechanisms.

My stack: [DESCRIBE, e.g., "Next.js on Vercel" / "WordPress on cPanel" /
  "Astro on Cloudflare Pages" / "Express on my own VPS" / "Hugo static site"]

Where my markdown lives: [pick one]
  - Same URL with .md suffix (e.g., /blog/my-post -> /blog/my-post.md)
  - Separate API path (e.g., /blog/my-post -> /api/markdown/my-post)
  - "I haven't built it yet"; tell me what to do first

Generate two things:

1. The <link rel="alternate" type="text/markdown" href="..."> tag to put in
   <head> of each HTML page, with the correct href pattern for my URLs.
   Show me how to inject it dynamically per page (template / layout / SSR
   helper) for my stack.

2. The HTTP Link: response header to send on every page, with the correct
   rel="alternate" and type="text/markdown" parameters. Include example
   middleware / route / CDN-rule code for my stack. The header should be
   set on the HTML response, not the markdown response.

Output:
- One code block per mechanism, copy-pasteable into my stack
- Plain-English explanation of where each goes
- Curl commands I can use to verify each signal is present
- A note on whether I should also implement content negotiation
  (Accept: text/markdown) for full coverage
```

## Heads up

- **The `<link>` tag and `Link:` header should agree.** Both should point at the same markdown URL for a given page. Mismatched targets confuse crawlers.
- **Per-page accuracy matters.** A site-wide template that links every page to `/llms.txt` is wrong: each HTML page should advertise *its own* markdown variant.
- **`type="text/markdown"` is non-negotiable.** Without it, the link reads as "an alternate version of this page" with no hint that it's machine-readable. Crawlers won't prefer it.
- **The `Link:` header is the underrated one.** Some headless AI agents never parse HTML at all; they request a URL, read response headers, and decide what to do next. The header is the only signal they see.
- **Pair with content negotiation.** The two markup signals tell clients *where* the markdown is. [Content negotiation](content-negotiation.md) lets them get markdown back from the same URL with no extra hop. Together they cover every flavor of client.
- **AI tooling is inconsistent today.** Some "AI fetch" tools ignore both signals and run an HTML→markdown conversion server-side anyway. You can't fix their bug, but shipping these signals means well-behaved tools take the fast path.

## Verify it's working

```bash
# <link> tag is in the rendered HTML
curl -s https://yoursite.com/some-page \
  | grep -i 'rel="alternate".*type="text/markdown"'
# Expect: <link rel="alternate" type="text/markdown" href="...">

# Link: header is in the response
curl -sI https://yoursite.com/some-page | grep -i '^link:'
# Expect: link: <https://yoursite.com/some-page.md>; rel="alternate"; type="text/markdown"

# The advertised target actually exists and returns markdown
TARGET=$(curl -s https://yoursite.com/some-page \
  | grep -oE 'rel="alternate" type="text/markdown" href="[^"]+"' \
  | grep -oE 'href="[^"]+"' \
  | sed 's/href="//; s/"$//')
curl -sI "$TARGET" | grep -iE "content-type|^http"
# Expect: HTTP/2 200 with content-type: text/markdown; charset=utf-8
```

## See also

- [Prompt: Content negotiation (`Accept: text/markdown`)](content-negotiation.md): the server-side companion; ship both for full coverage
- [Prompt: `.md` page routes](md-routes.md): the markdown URLs these signals point at
- [Prompt: `llms.txt`](llms-txt.md): a higher-level index AI consumers can follow into your `.md` routes
- [HTML spec: `<link>` element](https://html.spec.whatwg.org/multipage/links.html#link-type-alternate)
- [HTTP RFC 8288 (Web Linking / `Link:` header)](https://www.rfc-editor.org/rfc/rfc8288)
