# Contributing

Thanks for helping keep this current. The whole point is that no one person can track this space — it moves too fast.

## What good contributions look like

### Adding a new prompt

Drop a new file in [`prompts/`](prompts/). Follow [`templates/prompt.md`](templates/prompt.md). Required:

- File path, status badge, time-to-ship estimate
- One-paragraph context
- The prompt itself, in a code block
- "Heads up" — gotchas, platform issues
- "Verify it's working" — curl commands or tests
- Links to spec, examples, platform guides

Then add a row to the relevant table in [`README.md`](README.md) and an entry in [`prompts/README.md`](prompts/README.md).

### Adding a real-world example

Drop the file in `examples/[file-type]/[organization].txt` (or `.md`, `.json`). Real production files only — no toy examples. Add the source URL as a comment at the top of the file.

### Adding a platform guide

Drop a new file in `files/platforms/`. Match the structure of the existing platform guides ([`wordpress.md`](files/platforms/wordpress.md), [`shopify.md`](files/platforms/shopify.md)). Cover all the major files; if a file isn't supported on the platform, say so explicitly with workarounds.

### Updating status

If a provider starts or stops honoring a file, send a PR with a citation. Examples of what counts:

- Public statement from the provider (OpenAI, Anthropic, Google, etc.)
- Linked log analysis showing crawler behavior
- Spec ratification or deprecation
- Documented enterprise adoption

Vibes don't count. Ship the link.

### Correcting errors

Wrong adopter list, broken link, outdated spec version, typo — open a PR. No issue needed for small fixes.

### Removing files

If something's been formally deprecated or replaced, move it to a `## Deprecated` section in [`README.md`](README.md) rather than deleting outright. Add a note explaining what happened and what replaced it. People searching for the file should land somewhere that explains the change.

## What we won't merge

- Files with no spec or proposal — "I just made this up" doesn't count, no matter how clever
- Marketing-driven entries that aren't real standards
- Status upgrades without citations
- Prompts that hardcode a specific brand or product
- Anything that violates the [awesome-list](https://awesome.re/) ethos (vendor lock-in, paid placements, etc.)

## Status badge guide

| Badge | Means |
|---|---|
| ✅ Adopted | At least one major AI provider or platform formally honors it |
| ⚠️ Emerging | Real spec, real adopters, but no formal commitment from major providers |
| 🚀 New | Recently launched (under 12 months) with significant backing |
| 🚧 Coming soon | Spec in active drafting; not yet ratified or implemented |

When in doubt, downgrade. Better to undersell than mislead.

## Style

- Keep entries scannable. Tables for metadata, prose for context, code blocks for prompts
- Link to primary sources (specs, official announcements) over secondary sources (blog posts about specs)
- Be honest about adoption — `⚠️ Emerging` is not an insult
- Prompts should be platform-agnostic where possible. Use `[BRACKETS]` for placeholders the user fills in
- One file = one prompt = one entry. Don't combine multiple files in a single prompt.

## PR checklist

Before opening:

- [ ] Spec link works and points to the primary source
- [ ] Status badge reflects current real-world adoption
- [ ] Adopter list (if included) is accurate as of the PR date
- [ ] Prompt produces a working file when tested with at least one major AI tool
- [ ] No vendor advertisements
- [ ] Linked from `README.md` and `prompts/README.md` if it's a new prompt

## Code of conduct

Be decent. We're maintaining shared infrastructure, not winning arguments. Disagreements about file relevance or status should be resolved with citations, not volume.

## License

By contributing, you agree your contributions are released under [CC0](LICENSE).
