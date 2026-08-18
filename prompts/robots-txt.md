# Prompt: `robots.txt` (AI-aware)

**File:** `/robots.txt` · **Status:** ✅ Formal standard ([RFC 9309](https://www.rfc-editor.org/rfc/rfc9309)) · **Time to ship:** 15 min

The original, and still the first file every well-behaved AI crawler requests. Roughly 78% of top domains have one, but only about 16% carry any AI-specific rules ([Cloudflare Radar, April 2026](https://blog.cloudflare.com/agent-readiness/)) — most robots.txt files were written when Googlebot was the only serious reader.

The upgrade is to stop thinking "AI bots" and start thinking in four classes, because the right answer differs per class:

- **A. Answer engines & retrieval fetchers** (OAI-SearchBot, Claude-User, PerplexityBot…) — they cite and link back. Allow these or disappear from AI answers.
- **B. Model-training crawlers** (GPTBot, ClaudeBot, CCBot…) — the one genuinely philosophical decision. Either answer is fine; not knowing your answer isn't.
- **C. Coding agents & agentic browsers** (Claude-Code, Cursor, Devin…) — decides whether a developer's agent can read your docs on their behalf.
- **D. Bulk harvesters** (Bytespider, img2dataset, Scrapy…) — high volume, no citation, no traffic. The easy block.

**The #1 structural bug to avoid:** under RFC 9309, a crawler obeys *only* the most specific user-agent group that matches it. Name ClaudeBot in its own group and it stops reading your wildcard rules entirely. The fix: stack every allowed user-agent above ONE shared rule block. The prompt below enforces this.

---

## Copy this prompt

```
You are helping me write a robust, AI-agent-aware robots.txt file. Follow
RFC 9309 exactly, including group-merging rules: a crawler obeys only the
most specific matching user-agent group, so every allowed crawler must be
stacked above ONE shared rule block to inherit the same Disallow rules.

First, ask me for:

1. My site URL and what the site is (docs, SaaS, ecommerce, blog, media).
2. My audience: do developers use my site or docs? (Decides coding agents.)
3. My AI training stance: allow model-training crawlers (models learn my
   product/content by default) or block them (my content stays out of
   training sets). Explain the trade-off in two sentences before I choose,
   and note that blocking training does NOT affect AI answer visibility.
4. My AI answer visibility stance: nearly every site should allow answer
   engines and retrieval fetchers since they cite and link back. Confirm.
5. Crawl traps to close: ask me for my search URL pattern, faceted/filter
   params, action endpoints, session or tracking params that duplicate
   pages, and any infinite URL spaces (calendars, pagination bombs).
6. My sitemap situation: sitemap index URL if I have one, else the sitemap
   URL, else recommend generating one.
7. Whether I want a Content-Signal line (contentsignals.org syntax:
   search=..., ai-input=..., ai-train=...) matching my answers above.

Then, using web search if available, build the CURRENT list of user-agents
in four groups. My knowledge of bot names may be stale; verify against the
ai-robots-txt community list on GitHub (github.com/ai-robots-txt/ai.robots.txt,
165+ tracked user-agents as of August 2026) and each vendor's published
crawler docs:

  A. Answer engines & retrieval fetchers (e.g. OAI-SearchBot, ChatGPT-User,
     Claude-SearchBot, Claude-User, PerplexityBot, Perplexity-User,
     DuckAssistBot, Applebot, Gemini-Deep-Research, Google-NotebookLM,
     MistralAI-User, DeepSeekBot, Kimi-User, YouBot, ExaBot, TavilyBot
     and current peers)
  B. Model-training crawlers (e.g. GPTBot, ClaudeBot, CCBot,
     Google-Extended, Applebot-Extended, Meta-ExternalAgent, Amazonbot,
     bedrockbot, cohere-training-data-crawler, AI2Bot, PetalBot and
     current peers)
  C. Coding agents & agentic browsers (e.g. Claude-Code, Cursor, Devin,
     Operator, opencode, NovaAct, Trae, Manus-User and current peers)
  D. Bulk harvesters & scrapers with no citation value (e.g. Bytespider,
     TikTokSpider, img2dataset, LAIONDownloader, ImagesiftBot, Scrapy,
     aggressive SEO-tool bots)

Required output:

1. The complete robots.txt, structured as:
   - Header comment: site URL, RFC 9309 reference, link to my /llms.txt if
     I have one, and a one-line statement of the file's policy.
   - `User-agent: *` plus groups A–C (per my answers) stacked over ONE
     shared rule block: `Allow: /` followed by my crawl-trap Disallows,
     each with a trailing comment saying why it exists.
   - Group D stacked over `Disallow: /`.
   - My Content-Signal line if I opted in, with a comment.
   - The Sitemap line (index preferred).
   - A comment telling a future maintainer exactly which lines to move to
     flip the training decision.

2. A short "what this file says" summary in plain English, one line per
   group, so I can sanity-check the policy against my intent.

3. Verification steps: the curl commands to confirm the file is live and
   the specific user-agent + path combinations I should test with a
   robots.txt checker to confirm the group inheritance works as intended.

4. Flag anything I chose that contradicts itself (e.g. blocking retrieval
   bots while saying I want AI answer visibility) instead of silently
   complying.

Do not invent user-agent names. If you cannot verify a bot's current
token, say so and link to the vendor's crawler documentation instead.
```

## Heads up

- **The group-inheritance bug.** See above. If a testing tool shows a named bot ignoring your Disallow rules, this is almost always why.
- **OAI-SearchBot is for search, not training.** Same split everywhere: GPTBot trains, OAI-SearchBot/ChatGPT-User retrieve; ClaudeBot trains, Claude-SearchBot/Claude-User retrieve; Google-Extended gates training only and does not affect Google Search. Block the training bot, keep the retrieval bots, and you keep your AI answer visibility.
- **Cloudflare users:** Cloudflare's defaults now block many AI bots at the edge, before robots.txt is even a factor. Check your dashboard — a perfect robots.txt behind a bot challenge is a policy nobody reads.
- **robots.txt declares; it does not enforce.** Anything you truly need to prevent needs server-side controls (WAF, rate limits, auth).

## Verify it's working

```bash
# Live, plain text, no HTML shell
curl -I https://yoursite.com/robots.txt
# Should return 200 with Content-Type: text/plain

# Your AI bot groups are present
curl -s https://yoursite.com/robots.txt | grep -ci "user-agent"

# The bots you allowed can actually reach you end to end
curl -s -A "Mozilla/5.0 (compatible; ClaudeBot/1.0)" -o /dev/null -w "%{http_code}\n" https://yoursite.com/
```

Then test group inheritance with an RFC 9309 parser: your named crawlers should inherit the shared Disallow rules, and your blocked group should return disallowed for `/`.

## Updated bot list

The list of AI crawlers changes constantly — [ai-robots-txt](https://github.com/ai-robots-txt/ai.robots.txt) tracks 165+ user-agents and is the canonical reference. Check it before generating, and revisit your file quarterly.

## See also

- [Real production example: postman.com/robots.txt](https://www.postman.com/robots.txt) — four annotated bot classes, shared rule block, crawl traps closed
- [Real example: Anthropic's robots.txt](https://www.anthropic.com/robots.txt)
- [robots.txt in the AI files guide](https://www.tristandenyer.com/work/ai-files-for-websites-2026#1-robotstxt-updated-for-the-ai-era)
- [Platform guides for hosting `robots.txt`](../files/platforms/)
- [Spec: RFC 9309](https://www.rfc-editor.org/rfc/rfc9309) · [Content Signals Policy](https://contentsignals.org/) · [IETF AIPREF](https://datatracker.ietf.org/wg/aipref/about/)
