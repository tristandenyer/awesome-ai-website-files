# Prompt: `robots.txt` (AI-aware)

**File:** `/robots.txt` · **Status:** ✅ Honored · **Time to ship:** 5 min

The original. Most default configs don't mention AI bots. Add explicit rules.

---

## Copy this prompt

```
Generate an updated robots.txt for my website.

Site: [YOUR SITE URL]
Goal: [pick one]
  - Block all AI training crawlers, keep search engines
  - Allow AI crawlers (I want visibility in AI answers)
  - Allow search AI bots (OAI-SearchBot, etc.) but block training-only bots

Include explicit rules for these AI crawlers:
- OpenAI: GPTBot, ChatGPT-User, OAI-SearchBot
- Anthropic: ClaudeBot, anthropic-ai, Claude-Web
- Google: Google-Extended
- Perplexity: PerplexityBot, Perplexity-User
- Common Crawl: CCBot
- Apple: Applebot-Extended
- Meta: FacebookBot, Meta-ExternalAgent
- ByteDance: Bytespider
- Amazon: Amazonbot
- Cohere: cohere-ai
- Others: Diffbot, Omgilibot, ImagesiftBot, YouBot, Timpibot

Preserve a generic User-agent: * section for non-AI crawlers.
Add a Sitemap: line pointing to my sitemap.
Add comments explaining each section in plain English.
Return as a single code block I can copy directly into a file.
```

## Heads up

- **Cloudflare users:** Cloudflare changed defaults to block AI bots. Check your dashboard before assuming `robots.txt` is the only gate.
- **OAI-SearchBot is for search, not training.** If you block it, you won't appear in ChatGPT search results. Consider allowing it even if you block GPTBot.
- **Per-bot rules.** You can allow some, block others. Common pattern: allow search bots, block training-only bots.

## Verify it's working

```bash
curl -I https://yoursite.com/robots.txt
# Should return 200 with Content-Type: text/plain

curl https://yoursite.com/robots.txt | grep -i "gptbot\|claudebot\|google-extended"
# Should show your AI bot rules
```

## Updated bot list

The list of AI crawlers changes constantly. Check [ai-robots-txt](https://github.com/ai-robots-txt/ai.robots.txt) for the current canonical list before generating.

## See also

- [Real example: Anthropic's robots.txt](https://www.anthropic.com/robots.txt)
- [Platform guides for hosting `robots.txt`](../files/platforms/)
- [Spec: RFC 9309](https://www.rfc-editor.org/rfc/rfc9309)
