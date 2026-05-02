# Prompt: `.md` page routes

**File:** Same URLs as your HTML pages, with `.md` appended (e.g., `/blog/my-post` → `/blog/my-post.md`) · **Status:** ⚠️ Emerging · **Time to ship:** 1 to 4 hr depending on stack

The same URL with `.md` appended. So `yoursite.com/blog/my-post` gets a twin at `yoursite.com/blog/my-post.md` serving clean markdown.

This is what all the other AI visibility files point to. When a user pastes `yoursite.com/llms.txt` into ChatGPT, the AI follows the links, and if those links resolve to clean markdown instead of HTML soup, response quality jumps. A typical blog post goes from ~15,000 tokens of HTML down to ~3,000 tokens of markdown. That can be the difference between the AI understanding your page and giving up on it (or giving you a low-effort response).

Critical when humans paste your URLs into AI tools. Not yet auto-crawled, but Claude Code and Cursor fetch them today.

---

## Copy this prompt

```
I want to serve a clean markdown version of every page on my site at the same
URL with .md appended (e.g., /blog/my-post also available at /blog/my-post.md).

My site runs on: [pick one or describe]
  - WordPress / Next.js / Astro / Hugo / Ghost / Webflow / Squarespace
  - 11ty / Gatsby / Remix / SvelteKit / Nuxt
  - Custom Express / Rails / Django / Laravel / Go / etc.
  - "I don't know"; tell me how to find out

Content is currently stored as: [pick one]
  - Markdown files in the repo
  - A CMS database (WordPress, Sanity, Contentful, etc.)
  - HTML files
  - "I don't know"; tell me how to check

Give me:

1. The simplest way to set this up on my stack. Step-by-step, assume I can
   edit code but don't know anything about servers.

2. The route/handler code to serve markdown at the .md URL. The handler must:
   - Return Content-Type: text/markdown; charset=utf-8 (NOT text/plain)
   - Return a 200 (not a redirect to the HTML version)
   - Strip nav, footer, sidebar, ads. Content only.
   - Preserve heading structure, links, code blocks, lists
   - Include the page title as an H1 at the top
   - Include the canonical HTML URL as a comment or frontmatter field
   - Set sensible Cache-Control matching the HTML version's freshness

3. A way to handle pages that don't have a markdown source (e.g., generated
   pages, listings). Either skip them or render the visible content.

4. A test plan: a curl command to verify the .md route works, and a checklist
   of what to look for in the output.

Output:
- Code in a single block, copy-pasteable into my stack
- Plain-English explanation of where each piece goes
- Notes on any platform-specific gotchas
```

## Heads up

- **`Content-Type: text/markdown` matters.** Some stacks default to `text/plain` for `.md` files, which makes browsers download instead of display. Use `text/markdown; charset=utf-8`.
- **Don't redirect.** A 301 from `/page.md` to `/page` defeats the entire purpose. Serve the markdown directly with a 200.
- **Strip the chrome.** The whole reason this is faster than HTML→markdown conversion is that nav, footer, sidebar, cookie banners, share buttons, and analytics scripts are all gone. If your output has "Subscribe to our newsletter" boilerplate, you're doing it wrong.
- **Token math is real.** ~15K HTML tokens vs ~3K markdown tokens isn't a marketing number; measure your own pages with a tokenizer. Pages over a model's effective attention window get truncated; markdown buys you 4 to 5× more headroom.
- **Cache invalidation.** When you edit a page, both the HTML and `.md` variants need to invalidate together. Mismatched freshness is the most common bug after launch.
- **Static-site stacks are easiest.** Astro, Hugo, 11ty: usually a 5-line config change. Next.js needs a route handler. WordPress needs a plugin or theme function. Squarespace/Wix: not directly possible; host the markdown elsewhere and link to it from `llms.txt`.

## Verify it's working

```bash
# Markdown URL returns 200 with the right content-type
curl -sI https://yoursite.com/some-page.md | head -5
# Expect: HTTP/2 200
#         content-type: text/markdown; charset=utf-8

# Content is clean: no <nav>, <footer>, <script>, <aside>
curl -s https://yoursite.com/some-page.md | grep -iE "<nav|<footer|<script|<aside" || echo "clean ✓"

# Token count comparison (rough proxy: word count × ~1.3)
echo "HTML tokens (rough):"
curl -s https://yoursite.com/some-page | wc -w
echo "Markdown tokens (rough):"
curl -s https://yoursite.com/some-page.md | wc -w
```

## After you ship this

Pair it with the discovery mechanisms so AI clients can actually find these routes:

- [`markdown-discovery.md`](markdown-discovery.md): `<link rel="alternate" type="text/markdown">` in `<head>` and the matching HTTP `Link:` header
- [`content-negotiation.md`](content-negotiation.md): serve markdown from the same URL when the client sends `Accept: text/markdown`

Without discovery, only humans pasting URLs benefit. With discovery, headless agents (Claude Code, Cursor) fetch the markdown directly.

## See also

- [Prompt: Markdown link discovery](markdown-discovery.md): advertise these routes via `<link>` tag and `Link:` header
- [Prompt: Content negotiation](content-negotiation.md): serve markdown from the same URL via `Accept: text/markdown`
- [Prompt: `llms.txt`](llms-txt.md): link into your `.md` routes from a curated index
- [Platform guides](../files/platforms/): stack-specific instructions
- [llmstxt.org](https://llmstxt.org/): the convention these routes complement
