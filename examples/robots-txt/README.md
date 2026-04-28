# `robots.txt` examples

Real-world AI-aware `robots.txt` configurations.

## What's here

| File | Pattern | Use when |
|---|---|---|
| [`example-allow-search-block-training.txt`](example-allow-search-block-training.txt) | Allow AI search, block AI training | You want to appear in AI answers but not in training datasets |

## The three common patterns

**1. Allow everything (default for most sites).** Don't block any AI bots. Maximum visibility, contributes content to training.

**2. Allow AI search, block AI training** (the example here). The "best of both worlds" approach: you appear in ChatGPT/Perplexity answers but opt out of training datasets.

**3. Block all AI bots.** Maximum protection but you become invisible in AI answers. Common for premium publishers, paywalled content, copyrighted material.

## The critical distinction

People often conflate these:

- **Training bots** (`GPTBot`, `ClaudeBot`, `Google-Extended`) crawl to feed model training data
- **Search bots** (`OAI-SearchBot`, `ChatGPT-User`, `PerplexityBot`) fetch at query time to answer user questions

Block training bots → you stop contributing to model training. Your existing presence in trained models stays.

Block search bots → you become invisible in AI answers right now. This is usually not what people want.

Most sites should allow search bots and decide separately about training bots.

## Cloudflare warning

Cloudflare changed defaults to block AI bots at the network level. If your `robots.txt` says "allow" but you're on Cloudflare, check your Cloudflare dashboard. Your bot management settings may be overriding `robots.txt`.

## Adding more examples

PRs welcome. Save real production files as `examples/robots-txt/[organization].txt` with a header comment showing the source. Add a row to the table above.

See [CONTRIBUTING.md](../../CONTRIBUTING.md).
