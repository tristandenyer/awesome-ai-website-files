# Real-world examples

Production AI files from real sites, organized by file type. Each subfolder has its own README with patterns to copy and notes on what each example does well.

## What's here

| Folder | What's inside |
|---|---|
| [`llms-txt/`](llms-txt/) | `llms.txt` from Anthropic, Cloudflare, Stripe, and more |
| [`agents-md/`](agents-md/) | `AGENTS.md` patterns from production repos |
| [`robots-txt/`](robots-txt/) | AI-aware `robots.txt` configurations |
| [`tdmrep/`](tdmrep/) | `tdmrep.json` from major publishers (seeking contributions) |
| [`ai-txt/`](ai-txt/) | `ai.txt` (Spawning format) examples (seeking contributions) |

## How to use these

1. Pick the file type you're implementing
2. Open the folder and read the README — it lists the patterns each example demonstrates
3. Find the example closest to your situation (docs site → Anthropic; multi-product → Cloudflare; e-commerce with deprecated APIs → Stripe)
4. Pair the example with the matching prompt in [`prompts/`](../prompts/) and adapt to your site

## Adding examples

PRs welcome — examples are the highest-value contribution because everyone benefits from seeing real production patterns.

To add an example:

1. Save the file in the correct subfolder: `examples/[file-type]/[organization-or-pattern-name].[ext]`
2. Add a header comment with:
   - Source URL (where you fetched it from)
   - Date fetched
   - Any truncation notes if it's an excerpt of a larger file
3. Add a `# Why this is a notable example` section at the bottom explaining what's worth copying
4. Update the README in that subfolder with a row in the examples table
5. Open a PR

**Quality bar:** Real production files from real organizations only. Composite or illustrative examples are okay if clearly labeled as such (see [`agents-md/AGENTS.md`](agents-md/AGENTS.md) for an example of an honest composite).

See [CONTRIBUTING.md](../CONTRIBUTING.md) for full rules.

## A note on link rot

These files change. We try to keep examples representative, but the only canonical version is the live URL. Always check the source URL listed in each file's header for the current version.
