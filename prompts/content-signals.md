# Prompt: Content Signals in `robots.txt`

**File:** `/robots.txt` (directives) · **Status:** 🚀 New · ~4% of top domains · Cloudflare-backed, aligned with IETF AIPREF · **Time to ship:** 10 min

Content Signals are `Content-Signal:` directives inside your existing `robots.txt` that declare *how* fetched content may be used, separately from *whether* it may be fetched. Where a `Disallow:` line can only say "don't crawl," a Content Signal can say "search results yes, AI training no."

The vocabulary comes from the [Content Signals Policy](https://contentsignals.org/) (Cloudflare-backed, September 2025) and defines three signals:

- `search` — indexing for traditional search results
- `ai-input` — using content as input to AI answers (RAG, grounding, AI search)
- `ai-train` — using content to train or fine-tune models

This is the same direction the IETF AIPREF working group is standardizing, so declaring signals now is a low-cost bet on where preference expression is heading. Cloudflare measured roughly 4% adoption among top domains as of April 2026 — early, but growing, and Cloudflare injects it automatically for zones that opt in.

**Honest caveat:** like all of robots.txt, this is a declaration, not an enforcement mechanism. Pair it with server-side controls for anything you actually need to prevent.

---

## Copy this prompt

```
Add Content Signals to my robots.txt following the Content Signals Policy
(https://contentsignals.org/).

My site: [YOUR SITE URL]
My current robots.txt: [PASTE IT, OR "standard allow-all"]

My preferences (pick one per signal):
  - search: [yes / no]        (traditional search indexing)
  - ai-input: [yes / no]      (AI answers, RAG, AI search grounding)
  - ai-train: [yes / no]      (model training and fine-tuning)

A common default for sites that want AI visibility without donating
training data: search=yes, ai-input=yes, ai-train=no.

Required output:

1. My complete updated robots.txt with a Content-Signal line, e.g.:
     Content-Signal: search=yes, ai-input=yes, ai-train=no
   placed before the User-agent groups, plus the policy comment block
   from contentsignals.org explaining the vocabulary to human readers.

2. Keep every existing rule I pasted intact — Content Signals add to
   robots.txt, they don't replace crawl rules.

3. A one-paragraph explanation of what each signal I chose means and
   which AI companies have stated they honor it today.

4. A verification step: the exact line I should see when I fetch
   [MY SITE]/robots.txt after deploying.
```

---

## Verify it worked

```bash
curl -s https://yoursite.com/robots.txt | grep -i "content-signal"
```

You should see your `Content-Signal:` line. Then run the [AI Readiness Check](https://www.tristandenyer.com/ai-readiness-check) — the Content Signals card should flip to a pass.

## Spec and further reading

- [Content Signals Policy](https://contentsignals.org/)
- [Cloudflare: Introducing the Agent Readiness Score](https://blog.cloudflare.com/agent-readiness/)
- [IETF AIPREF working group](https://datatracker.ietf.org/wg/aipref/about/) — the standards-track successor
