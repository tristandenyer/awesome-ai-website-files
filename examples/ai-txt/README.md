# `ai.txt` examples

Real-world `ai.txt` files (Spawning.ai format).

## Status

🚧 This folder is being seeded. PRs welcome.

## What `ai.txt` is

The Spawning.ai `ai.txt` format declares your AI training preferences. Honored by Spawning's network, which includes Hugging Face and Stability AI.

Format reference: [site.spawning.ai/spawning-ai-txt](https://site.spawning.ai/spawning-ai-txt)

## Where to look for examples

Spawning maintains a registry of opted-out sites. Common places to find `ai.txt`:

- Artist portfolios that have publicly opted out
- Photographer and illustrator personal sites
- Stock content sites with restrictive licensing
- News publishers using Spawning's tools

To check any site:

```bash
curl https://example.com/ai.txt
```

## Naming collision warning

There are at least two competing `ai.txt` specifications:

1. **Spawning.ai's `ai.txt`** (this folder): about training opt-out
2. **ai-visibility.org.uk's `ai.txt`** (ADF-004): about behavioral guidance

If you submit an example, **make clear which spec it follows.** Spawning's version is the one with real adoption.

## Adding examples

PRs welcome. Submit real production files with a header comment showing the source URL and which spec it follows.

See [CONTRIBUTING.md](../../CONTRIBUTING.md).
