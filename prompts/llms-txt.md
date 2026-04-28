# Prompt: `llms.txt`

**File:** `/llms.txt` · **Status:** ⚠️ Emerging · **Time to ship:** 5 min

Curated markdown index of your most important content. Used by ChatGPT, Claude, Cursor, and other tools when given the URL. Not yet auto-crawled by major providers.

---

## Copy this prompt

```
Generate an llms.txt for my website following the spec at https://llmstxt.org/.

Site: [YOUR SITE URL]
What my site is about (one sentence): [DESCRIBE]
Audience: [WHO IT'S FOR — e.g., "developers building with our API", 
"parents looking for recipes", "small business owners"]

Most important pages (5-15, with URLs and one-line descriptions):
- [URL]: [what it is]
- [URL]: [what it is]
- ...

Required format:
- Single # H1 with my site name
- > blockquote summary (one sentence)
- ## H2 sections grouping related links (e.g., "Documentation", "Blog", 
  "Products", "About")
- Each link as: - [Page Title](URL): short description
- ## Optional section at the end for secondary/nice-to-have pages

Return the full file as one code block I can save at /llms.txt.
```

## Heads up

- **No AI crawler reliably fetches this unprompted as of early 2026.** Real value comes when humans paste your URL into ChatGPT, or when coding agents (Cursor, Claude Code) follow links.
- **Quality over quantity.** Mintlify recommends keeping it under 50KB. Don't dump every URL — curate.
- **Pair with `.md` routes.** The links in your `llms.txt` work much better when they resolve to clean markdown. See [`md-routes.md`](md-routes.md).

## Verify it's working

```bash
curl -I https://yoursite.com/llms.txt
# Should return 200 with Content-Type: text/plain or text/markdown
```

Then drop your URL into ChatGPT or Claude and ask it to summarize your site. If it picks up the structure, you're set.

## Generators

If you don't want to write it from scratch, use:

- [Firecrawl llms.txt generator](https://llmstxt.firecrawl.dev/)
- [Mintlify llms.txt generator](https://www.mintlify.com/tools/llmstxt)
- [Wordlift LLMs.txt Generator](https://wordlift.io/llms-txt-generator/)
- WordPress: [Yoast SEO](https://yoast.com/) or [Rank Math](https://rankmath.com/) auto-generate

## See also

- [Real examples: Anthropic, Stripe, Cloudflare, Vercel](../examples/llms-txt/)
- [Platform guides — WordPress, Shopify, Squarespace, etc.](../files/platforms/)
- [Spec: llmstxt.org](https://llmstxt.org/)
- [llms-full.txt prompt](llms-full-txt.md) — the fat companion file
