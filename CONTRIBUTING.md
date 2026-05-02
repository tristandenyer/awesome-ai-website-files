# Contributing

Thanks for helping keep this current. The whole point is that no one person can track this space; it moves too fast.

## What good contributions look like

### Adding a new prompt

Drop a new file in [`prompts/`](prompts/). Follow [`templates/prompt.md`](templates/prompt.md). Required:

- File path, status badge, time-to-ship estimate
- One-paragraph context
- The prompt itself, in a code block
- "Heads up" section covering gotchas and platform issues
- "Verify it's working" section with curl commands or tests
- Links to spec, examples, platform guides

Then add a row to the relevant table in [`README.md`](README.md) and an entry in [`prompts/README.md`](prompts/README.md).

### Adding a real-world example

Drop the file in `examples/[file-type]/[organization].txt` (or `.md`, `.json`). Real production files only; no toy examples. Add the source URL as a comment at the top of the file.

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

Wrong adopter list, broken link, outdated spec version, typo: open a PR. No issue needed for small fixes.

### Removing files

If something's been formally deprecated or replaced, move it to a `## Deprecated` section in [`README.md`](README.md) rather than deleting outright. Add a note explaining what happened and what replaced it. People searching for the file should land somewhere that explains the change.

## What we won't merge

- Files with no spec or proposal. "I just made this up" doesn't count, no matter how clever.
- Marketing-driven entries that aren't real standards
- Status upgrades without citations
- Prompts that hardcode a specific brand or product
- Anything that violates the [awesome-list](https://awesome.re/) ethos (vendor lock-in, paid placements, etc.)

## Status badge guide

| Badge          | Means                                                                   |
| -------------- | ----------------------------------------------------------------------- |
| ✅ Adopted     | At least one major AI provider or platform formally honors it           |
| ⚠️ Emerging    | Real spec, real adopters, but no formal commitment from major providers |
| 🚀 New         | Recently launched (under 12 months) with significant backing            |
| 🚧 Coming soon | Spec in active drafting; not yet ratified or implemented                |

When in doubt, downgrade. Better to undersell than mislead.

## Style

- Keep entries scannable. Tables for metadata, prose for context, code blocks for prompts
- Link to primary sources (specs, official announcements) over secondary sources (blog posts about specs)
- Be honest about adoption. `⚠️ Emerging` is not an insult.
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

## AI Prompt to get started

Pick the prompt that matches what you're contributing. Paste it into Claude, ChatGPT, or any other LLM. The output is a starting draft, not a finished PR. You still need to verify facts, add citations, and run the PR checklist above.

### A) Adding a new prompt file

```
I'm contributing a new prompt to the awesome-ai-website-files repo
(https://github.com/tristandenyer/awesome-ai-website-files).

The repo's contribution rules are at CONTRIBUTING.md. The prompt template is
at templates/prompt.md. Existing prompts in prompts/ show the expected style.

The file I'm adding a prompt for:
  Name: [FILE NAME, e.g., "robots.txt", "tdmrep.json", "/.well-known/foo.json"]
  Path on a website: [WHERE IT LIVES, e.g., /robots.txt]
  Spec or proposal URL: [REQUIRED: primary source]
  Status (✅ / ⚠️ / 🚀 / 🚧): [PICK ONE per the badge guide]
  Known adopters: [LIST; required for ✅ and ⚠️]
  Why doers should care: [1-2 SENTENCES]

Generate a complete prompts/[file-slug].md following the template exactly:
- Header line with file path, status, time-to-ship
- One-paragraph context (concrete, brief)
- "Copy this prompt" code block using [BRACKETS] for user inputs
- "Heads up" with 3-5 specific gotchas (cite where possible)
- "Verify it's working" with curl commands
- "See also" linking to spec, examples, related prompts

Match the prose style of existing files in prompts/ (technical, terse,
no marketing language). Do not invent adopters or status. If I left a
field blank, ask me before guessing.
```

### B) Adding a real-world example

```
I'm contributing a real production example to the awesome-ai-website-files
repo. Examples live in examples/[file-type]/ and document patterns worth
copying.

The example I'm adding:
  File type: [robots-txt / llms-txt / agents-md / tdmrep / ai-txt / other]
  Source organization: [WHO PUBLISHES IT]
  Source URL: [WHERE I FETCHED IT]
  Date fetched: [YYYY-MM-DD]
  Why it's notable (1-2 sentences): [WHAT PATTERN DOES IT DEMONSTRATE]

Generate two things:

1. The example file itself, ready to drop in examples/[file-type]/[slug].[ext]:
   - Header comment block at the top with: source URL, date fetched, any
     truncation notes if it's an excerpt
   - The actual content (paste-ready, exactly as fetched)
   - A "# Why this is a notable example" section at the bottom explaining
     what's worth copying

2. The README row to add to examples/[file-type]/README.md, matching the
   existing table format.

Real production files only. If I haven't given you the actual content,
ask for it. Do not fabricate.
```

### C) Adding a platform guide

```
I'm contributing a platform guide to the awesome-ai-website-files repo.
Platform guides live in files/platforms/ and explain how to ship every AI
file on a specific stack.

Reference existing guides: files/platforms/wordpress.md and
files/platforms/shopify.md. Match their structure exactly.

The platform I'm covering:
  Name: [e.g., Webflow, Astro, Squarespace, Next.js]
  Hosting model: [e.g., managed PaaS / self-host / static / SaaS]
  Root file access: [yes / no / via workaround]

For each AI file in the repo (robots.txt, llms.txt, llms-full.txt,
.md routes, schema.org JSON-LD, ai.txt, tdmrep.json, AI meta tags,
NLWeb, MCP/A2A agent files, AGENTS.md), tell me:
  - Native support? (yes / no / partial)
  - Recommended approach (plugin name, code snippet, config change)
  - Workaround if not natively supported
  - One platform-specific gotcha

Output a complete files/platforms/[platform-slug].md file matching the
shape of wordpress.md. Include a "Verification" section with curl
commands and a "Heads up" section with platform-specific caveats
(caching, theme overrides, .well-known support, etc.).

If I left a field blank, ask before guessing. Do not invent plugins or
features. Link to real, current docs.
```

After the AI generates a draft, run through the **PR checklist** above before opening the PR. Spec links must work, status must reflect real adoption, no fabricated adopters or plugins.

## Code of conduct

Be decent. We're maintaining shared infrastructure, not winning arguments. Disagreements about file relevance or status should be resolved with citations, not volume.

## License

By contributing, you agree your contributions are released under [CC0](LICENSE).
