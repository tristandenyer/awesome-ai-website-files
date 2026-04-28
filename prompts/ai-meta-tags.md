# Prompt: AI meta tags & `X-Robots-Tag`

**File:** HTML `<head>` meta tags + `X-Robots-Tag` HTTP response header · **Status:** ⚠️ Informally honored · **Time to ship:** 15 to 30 min

Per-page signals you put directly in the HTML `<head>` (or send as an HTTP header) to tell AI crawlers what to do with that specific page.

`robots.txt` controls crawling at the site level. Meta tags let you override that at the page level. The IPTC (the international press body) recommends combining `noarchive`, `noai`, and `noimageai` directives for publishers who want to remain visible in search but stay out of AI training. DeviantArt popularized the `X-Robots-Tag` HTTP header to apply these rules at scale without touching every page's HTML.

`noai` and `noimageai` are not formal standards. They're conventions some AI companies respect and others ignore. They are still the most granular, per-page signal available today.

---

## Copy this prompt

```
Show me the exact HTML meta tags I should add to the <head> of pages on my site
to signal AI preferences, following IPTC recommendations.

My goal: [pick one]
  - Block AI training but allow search engines (most common: ship in search,
    stay out of training data)
  - Block AI training AND archiving (ChatGPT, Bard cached snapshots, Wayback)
  - Allow everything (you only need this if you're explicitly opting in)
  - Block image AI training only (text is fine)

My stack: [DESCRIBE, e.g., "Next.js" / "WordPress with Yoast" /
  "static Hugo site" / "Cloudflare in front of origin"]

Generate:

1. The exact <meta name="robots" content="..."> tags to put in <head>.
   Include separate tags as needed for: noai, noimageai, noarchive, noindex,
   nosnippet. Do NOT combine directives that target different things into one tag
   if it reduces clarity.

2. The equivalent X-Robots-Tag HTTP response header versions, for people who
   configure this at the server/CDN level (preferred at scale because you can
   apply by path pattern without editing HTML).

3. Stack-specific snippets:
   - For my stack above, show me where to add these (theme function, _document,
     middleware, vercel.json, .htaccess, Cloudflare Worker, etc.)
   - If my stack has a popular SEO plugin (Yoast, Rank Math, next-seo, astro-seo),
     show me how to do it via that plugin first.

4. A brief plain-English explanation of what each directive does and which AI
   companies are known to honor it (cite sources where possible).

Output:
- Each piece in a separate code block
- A note on which directives are formal standards (noindex, noarchive) vs
  conventions (noai, noimageai)
```

## Heads up

- **`noai` and `noimageai` are conventions, not standards.** OpenAI, Anthropic, and Google have made varying public statements; none have committed to honoring them in the same way they honor `robots.txt`. Treat as best-effort signaling.
- **Use `X-Robots-Tag` for scale.** If you have a thousand pages, don't edit every template; set the header at your CDN or server. One Cloudflare Worker rule beats a thousand template edits.
- **`noarchive` is real and widely honored.** It's a formal Google/Bing directive. Combined with `noai`, you get "show me in search, don't cache me, don't train on me." That's the IPTC-recommended combination for publishers.
- **Don't ship `noindex` by accident.** `noindex` removes you from search entirely. Many people confuse it with "don't train on me." If you want AI training opt-out without losing search traffic, use `noai`/`noimageai`/`noarchive` and leave `noindex` off.
- **This is not enforcement.** Meta tags are signals, not access control. If you need to legally prevent training in the EU, see [`tdmrep-json`](tdmrep-json.md); that's the file with actual legal weight.

## Verify it's working

```bash
# Check meta tags rendered in HTML
curl -s https://yoursite.com/some-page \
  | grep -iE 'name="robots"|noai|noimageai|noarchive'

# Check X-Robots-Tag is sent as a response header
curl -sI https://yoursite.com/some-page | grep -i x-robots-tag
# Expect: x-robots-tag: noai, noimageai, noarchive (or your chosen combination)
```

If you set both, the meta tag and the header should agree. Conflicting signals (header says `noai`, meta says nothing) get resolved differently by different crawlers, so be consistent.

## See also

- [IPTC Generative AI Opt-Out Best Practices (PDF)](https://iptc.org/std/guidelines/data-mining-opt-out/IPTC-Generative-AI-Opt-Out-Best-Practices.pdf)
- [Google: robots meta tag and X-Robots-Tag specifications](https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag)
- [Prompt: `robots.txt`](robots-txt.md): site-level rules; pair with this for layered control
- [Prompt: `tdmrep.json`](tdmrep-json.md): EU legal-weight opt-out
- [Prompt: `ai.txt`](ai-txt-spawning.md): Spawning.ai network opt-out
