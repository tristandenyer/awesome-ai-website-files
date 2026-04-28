# Prompts

One file per prompt. Each file is self-contained: prompt, gotchas, verification, real examples.

## Permission files

- [`robots-txt.md`](robots-txt.md): AI-aware robots.txt
- [`ai-txt-spawning.md`](ai-txt-spawning.md): Spawning.ai opt-out
- [`tdmrep-json.md`](tdmrep-json.md): W3C TDM Reservation Protocol (legal weight in EU)
- [`ai-meta-tags.md`](ai-meta-tags.md): `noai` / `noimageai` meta tags + `X-Robots-Tag`

## Visibility files

- [`llms-txt.md`](llms-txt.md): curated content index for AI
- [`llms-full-txt.md`](llms-full-txt.md): full content concatenation (3-4× more traffic than llms.txt)
- [`md-routes.md`](md-routes.md): clean markdown at `.md` URLs
- [`markdown-discovery.md`](markdown-discovery.md): `<link>` tag, HTTP `Link` header, content negotiation
- [`ai-hint-div.md`](ai-hint-div.md): visually hidden hint for paste-into-AI
- [`schema-jsonld.md`](schema-jsonld.md): schema.org structured data

## Agent files

- [`nlweb.md`](nlweb.md): Microsoft NLWeb implementation plan
- [`agent-card-json.md`](agent-card-json.md): A2A protocol agent card
- [`mcp-json.md`](mcp-json.md): MCP server manifest

## Coding agent files

- [`agents-md.md`](agents-md.md): instructions for AI coding tools

## How prompts are structured

Each prompt file follows the same shape so doers know exactly where to look:

1. **Header:** file path, status, time to ship
2. **One-paragraph context:** what this is, when to use it
3. **Copy this prompt:** the prompt block, ready to paste
4. **Heads up:** gotchas, platform issues, common mistakes
5. **Verify it's working:** curl commands or tests to confirm
6. **See also:** links to examples, platform guides, the spec

If you're adding a new prompt, follow the same shape. See [`templates/prompt.md`](../templates/prompt.md).
