# Prompt: `llms-full.txt`

**File:** `/llms-full.txt` · **Status:** ⚠️ Emerging, but meaningfully used by ChatGPT and coding agents · **Time to ship:** 1 to 4 hr

The fat companion to `llms.txt`. While `llms.txt` is just a curated index with links, `llms-full.txt` is your entire site's content concatenated into a single markdown file. Not part of the formal spec, but a widely adopted convention popularized by Mintlify.

An AI can ingest your entire documentation set in one fetch: no crawling, no context-window juggling. Mintlify has reported that `llms-full.txt` actually gets **3 to 4 times more traffic than `llms.txt`**, with ChatGPT driving the majority of that. For documentation sites, this is the file that likely gets read.

**When to skip:** for marketing sites, personal blogs, or sites under ~20 pages, just redirect `/llms-full.txt` to `/llms.txt` or `/index.md`. This file shines for documentation-heavy sites.

---

## Copy this prompt

```
Help me plan and generate an llms-full.txt file for my site.

My site: [YOUR SITE URL]
Type of site: [pick one]
  - documentation site
  - knowledge base
  - blog
  - marketing site
  - other: [DESCRIBE]
Approximate number of pages: [NUMBER]
Total approximate word count across all pages: [ESTIMATE, or "I don't know"]

Step 1: Tell me whether llms-full.txt is worth it for my type/size of site,
or whether I should just redirect /llms-full.txt to /llms.txt or /index.md.
Be honest. For sub-20-page marketing sites, it's usually overkill.

Step 2: If it's worth it, give me:
- A recommended structure (table of contents, then full content of each page)
- A script or workflow I can use to automatically generate it from my existing
  pages (tell me which tech stack you're assuming, ask me if unsure)
- A reasonable size target (aim for under 500KB unless I'm Cloudflare-scale)
- Cache and refresh strategy (regenerate on deploy, on schedule, or on demand?)

Step 3: Output a starter template I can fill in, showing the header format
and how sections should be organized.

Required format:
- Plain markdown, served as text/markdown; charset=utf-8
- Single # H1 with site name at the top
- A > blockquote summary
- Optional table of contents
- Each page as a ## H2 section with the source URL noted
- Code blocks preserved verbatim
- No HTML tags, no nav/footer chrome
```

## Heads up

- **Size discipline matters.** Files over ~500KB get truncated by some AI clients before they finish reading. If your full corpus is bigger, split into topical `llms-full-{topic}.txt` files and link from `llms.txt`.
- **It's a convention, not a spec.** Unlike `llms.txt`, there's no formal spec at llmstxt.org for this file. Mintlify popularized the format; everyone else copied it. Reasonable to follow Mintlify's structure.
- **Don't include your blog archive from 2014.** This file is consumed in one shot. Curate ruthlessly. Put canonical, evergreen content in; leave news/changelogs out unless the changelog _is_ the product.
- **Regenerate on deploy.** If you generate this file once and forget, it becomes a stale snapshot of your docs. Bake regeneration into your build pipeline.
- **Marketing sites should skip.** A 12-page marketing site doesn't benefit. Redirect `/llms-full.txt` → `/llms.txt`.

## Verify it's working

```bash
# Confirm it's served as markdown, not HTML
curl -sI https://yoursite.com/llms-full.txt | grep -i content-type
# Expect: content-type: text/markdown; charset=utf-8

# Check size; under 500KB is the safe zone
curl -sI https://yoursite.com/llms-full.txt | grep -i content-length
# Or: curl -s https://yoursite.com/llms-full.txt | wc -c

# Spot-check the structure
curl -s https://yoursite.com/llms-full.txt | head -30
# Expect: H1, blockquote summary, then ## H2 sections
```

After it ships, watch your server logs for fetches by `ChatGPT-User`, `Claude-User`, and referrers from `chatgpt.com` / `claude.ai`. That's how you know it's actually being consumed.

## See also

- [Prompt: `llms.txt`](llms-txt.md): the curated index this file complements
- [Prompt: `.md` page routes](md-routes.md): what `llms.txt` should link to
- [Mintlify: how to generate `llms-full.txt`](https://www.mintlify.com/blog/how-to-generate-llmstxt-file-automatically)
- [Real examples](../examples/llms-txt/): Anthropic, Stripe, Cloudflare ship both files
