# Prompt: `ai.txt` (Spawning.ai)

**File:** `/ai.txt` · **Status:** ⚠️ Partially honored, adopted by Spawning's network · **Time to ship:** 5 min

A plain-text opt-out specifically for generative AI training. Proposed by [Spawning.ai](https://spawning.ai/) and honored by their partners, including Hugging Face and Stability AI.

`robots.txt` controls _crawling_. `ai.txt` controls _training use_. The distinction matters: content can end up in training datasets via third-party scrapes even after you tighten `robots.txt`. `ai.txt` is checked when someone re-downloads your content from a dataset, giving you a real-time opt-out. It also explicitly targets the EU's TDM Article 4 exception, giving you a legal hook.

Not honored by OpenAI, Anthropic, Google, or Meta directly today. Worth the 5 minutes anyway, since Spawning's network is real, and it pairs naturally with [`tdmrep.json`](tdmrep-json.md).

**Naming-confusion warning:** there's a second, unrelated "ai.txt" spec from ai-visibility.org.uk that's about behavioral guidance (what AI systems should say about you), _not_ training opt-out. The Spawning version is the one with real adoption. Don't mix them up.

---

## Copy this prompt

```
Generate an ai.txt file for my website in the Spawning.ai format.

My site: [YOUR SITE URL]
My preference: [pick one]
  - Opt out of all AI training (text, image, video, audio, code)
  - Opt out only of image training
  - Opt out only of text training
  - Allow AI training with attribution required
  - Allow AI training (you only need this if explicitly opting in)

Cover these media categories: text, images, video, audio, code.
Follow the Spawning.ai ai.txt spec (https://site.spawning.ai/spawning-ai-txt).

Required output:

1. The ai.txt file content, as plain text. Each rule on its own line, using
   the "User-Agent: ... Disallow: ..." style the spec defines for each
   media category.

2. Served at /ai.txt with:
   - Content-Type: text/plain; charset=utf-8
   - 200 status (not a redirect)
   - Cacheable (this rarely changes)

3. A short comment block at the top in plain English explaining what the
   file declares, so a human reading it can understand without the spec.

4. Note any fields the spec defines that I'm not using and why I might want
   them later (e.g., expiration dates, contact info).

Return as a single code block.
```

## Heads up

- **Spawning's network is the actual adoption.** Hugging Face datasets and Stability AI honor it via Spawning's API. Major foundation-model labs (OpenAI, Anthropic, Google, Meta) do not currently check `ai.txt` directly, so don't assume site-wide AI training opt-out from this file alone.
- **Pair with `tdmrep.json`.** TDMRep gives you EU legal standing; `ai.txt` gives you Spawning-network coverage. Together they're the strongest opt-out signal you can ship today.
- **Real-time opt-out is the under-appreciated win.** Even after a model is trained on your old content, Spawning's API checks `ai.txt` again at re-download time. That means tightening this file affects the next training cycle, not just future scrapes.
- **Don't confuse with `/.well-known/ai.txt`.** Multiple unrelated proposals exist at that path. The Spawning file lives at `/ai.txt` (root), not under `/.well-known/`.
- **Categories are independent.** You can opt out of image training while allowing text training. Useful for stock photo sites, illustrators, photographers who want their words findable but their pictures untouched.

## Verify it's working

```bash
# Confirm it's served correctly
curl -sI https://yoursite.com/ai.txt | head -5
# Expect: HTTP/2 200
#         content-type: text/plain; charset=utf-8

# Spot-check the rules
curl -s https://yoursite.com/ai.txt
# Expect: User-Agent / Disallow lines per media category
```

Spawning maintains [Have I Been Trained?](https://haveibeentrained.com/) as a way to check what's in major datasets and submit opt-outs.

## See also

- [Spec: Spawning.ai ai.txt](https://site.spawning.ai/spawning-ai-txt)
- [Spawning API docs](https://api.spawning.ai/)
- [Prompt: `tdmrep.json`](tdmrep-json.md): pair for legal weight in EU
- [Prompt: `robots.txt`](robots-txt.md): controls crawling; this controls training
- [Prompt: AI meta tags](ai-meta-tags.md): per-page signaling
