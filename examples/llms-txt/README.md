# `llms.txt` examples

Real production `llms.txt` files from major sites, with notes on what each does well.

## What's here

| File | Source | Pattern | Why it's notable |
|---|---|---|---|
| [`anthropic.txt`](anthropic.txt) | docs.anthropic.com | Clean catalog | Tight index (~8K tokens) paired with a larger llms-full.txt for ingestion |
| [`cloudflare-workers.txt`](cloudflare-workers.txt) | developers.cloudflare.com | Product split | Multi-product company splits llms.txt by service area |
| [`stripe.txt`](stripe.txt) | stripe.com | Instructions block | Includes explicit AI-coding instructions, not just an index |

## Patterns to copy

**Clean catalog (Anthropic):** for documentation sites. Tight curated index, with descriptive text on each link, and heavy use of the `## Optional` section to deprioritize secondary content. Pair with a `llms-full.txt` for ingestion.

**Product split (Cloudflare):** for multi-product companies. Don't dump everything into one file. Split by service area (`/workers/llms.txt`, `/ai/llms.txt`, etc.) so AI agents only fetch the section relevant to what they're building.

**Instructions block (Stripe):** for any site with deprecated APIs or evolving best practices. Add an explicit "Instructions for Code-Writing AI" section near the top telling AI assistants what to recommend and what to avoid. This actively corrects bias from older training data.

## How to use these

1. Pick the pattern closest to your site type (docs / multi-product / commerce / etc.)
2. Read the `# Why this is a notable example` section at the bottom of each file
3. Use the [`llms-txt` prompt](../../prompts/llms-txt.md) and reference the pattern you want to follow
4. Verify your file at `yoursite.com/llms.txt`

## Heads up

These examples are **excerpts** of the live files (the real Cloudflare files are millions of tokens). Always fetch the live URL listed at the top of each file for the current canonical version. These update frequently.

## Adding more examples

PRs welcome. To add an example:

1. Save the file as `examples/llms-txt/[organization].txt`
2. Add a header comment with the source URL and date fetched
3. Add a `# Why this is a notable example` section at the bottom explaining what's worth copying
4. Add a row to the table above
5. Make sure it's a *real* production file from a *real* organization, not a constructed example

See [CONTRIBUTING.md](../../CONTRIBUTING.md) for the full contribution rules.
