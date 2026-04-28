# Awesome AI Website Files

> All the AI files your website should have to be crawlable, parsable, discoverable, and to drive traffic — with copy-paste prompts to generate each one.

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md) [![License: CC0](https://img.shields.io/badge/License-CC0-blue.svg)](LICENSE)

📖 **[Read the full guide →](https://www.tristandenyer.com/work/ai-files-for-websites-2026)** for the why, the context, and the full breakdown.

This repo is the **doer's version**: skip to the file you need, grab the prompt, ship it.

---

## 🚀 Pick your path

**I have 15 minutes** → Do the [Quick Start Three](#quick-start-three).

**I want one specific file** → Jump to [The List](#the-list).

**I want all the prompts in one place** → [`prompts/`](prompts/).

**I want to see real examples from real sites** → [`examples/`](examples/).

**I run a [WordPress / Shopify / Squarespace] site** → See [platform guides](#platform-guides).

**I want to contribute** → [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Quick Start Three

If you do nothing else, do these three. Total time: ~15 minutes.

| #   | File                    | Time  | AI prompt                               |
| --- | ----------------------- | ----- | ------------------------------------ |
| 1   | `robots.txt` (AI-aware) | 5 min | [→ prompt](prompts/robots-txt.md)    |
| 2   | `llms.txt`              | 5 min | [→ prompt](prompts/llms-txt.md)      |
| 3   | `schema.org` JSON-LD    | 5 min | [→ prompt](prompts/schema-jsonld.md) |

Developers, add: [`AGENTS.md`](prompts/agents-md.md) in your repo root.

---

## The List

Status: ✅ Adopted · ⚠️ Emerging · 🚀 New · 🚧 Coming soon

### Permission files — control AI training and crawling

| File                | Status | Path                       | AI prompt                           | Spec                                                                                                      |
| ------------------- | ------ | -------------------------- | -------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `robots.txt`        | ✅     | `/robots.txt`              | [View →](prompts/robots-txt.md)      | [RFC 9309](https://www.rfc-editor.org/rfc/rfc9309)                                                        |
| `ai.txt` (Spawning) | ⚠️     | `/ai.txt`                  | [View →](prompts/ai-txt-spawning.md) | [spawning.ai](https://site.spawning.ai/spawning-ai-txt)                                                   |
| `tdmrep.json`       | ✅ EU  | `/.well-known/tdmrep.json` | [View →](prompts/tdmrep-json.md)     | [W3C](https://w3c.github.io/tdm-reservation-protocol/spec/)                                               |
| AI meta tags        | ⚠️     | HTML `<head>`              | [View →](prompts/ai-meta-tags.md)    | [IPTC](https://iptc.org/std/guidelines/data-mining-opt-out/IPTC-Generative-AI-Opt-Out-Best-Practices.pdf) |

### Visibility files — help AI find and understand your content

| File                    | Status | Path               | AI prompt                              | Spec                                                                                   |
| ----------------------- | ------ | ------------------ | ----------------------------------- | -------------------------------------------------------------------------------------- |
| `llms.txt`              | ⚠️     | `/llms.txt`        | [View →](prompts/llms-txt.md)           | [llmstxt.org](https://llmstxt.org/)                                                    |
| `llms-full.txt`         | ⚠️     | `/llms-full.txt`   | [View →](prompts/llms-full-txt.md)      | [convention](https://www.mintlify.com/blog/how-to-generate-llmstxt-file-automatically) |
| `.md` page routes       | ⚠️     | `/page.md`         | [View →](prompts/md-routes.md)          | [llmstxt.org](https://llmstxt.org/)                                                    |
| Markdown link discovery | ✅     | `<head>` + headers | [View →](prompts/markdown-discovery.md) | HTTP                                                                                   |
| Hidden AI hint div      | ⚠️     | `<body>`           | [View →](prompts/ai-hint-div.md)        | convention                                                                             |
| `schema.org` JSON-LD    | ✅     | `<script>` block   | [View →](prompts/schema-jsonld.md)      | [schema.org](https://schema.org)                                                       |

### Agent files — make your site queryable by AI agents

| File                    | Status | Path                           | AI prompt                           | Spec                                                  |
| ----------------------- | ------ | ------------------------------ | -------------------------------- | ----------------------------------------------------- |
| NLWeb                   | 🚀     | `/ask` endpoint                | [View →](prompts/nlweb.md)           | [microsoft/NLWeb](https://github.com/microsoft/NLWeb) |
| `agent-card.json` (A2A) | 🚀     | `/.well-known/agent-card.json` | [View →](prompts/agent-card-json.md) | [A2A](https://a2a-protocol.org/)                      |
| `mcp.json`              | 🚀     | `/.well-known/mcp.json`        | [View →](prompts/mcp-json.md)        | [MCP](https://modelcontextprotocol.io/)               |

### Coding agent files — for repos, not websites

| File        | Status | Path         | AI prompt                     | Spec                            |
| ----------- | ------ | ------------ | -------------------------- | ------------------------------- |
| `AGENTS.md` | ✅     | `/AGENTS.md` | [View →](prompts/agents-md.md) | [agents.md](https://agents.md/) |

### Coming soon

| File                   | Status | Notes                                                                                                                      |
| ---------------------- | ------ | -------------------------------------------------------------------------------------------------------------------------- |
| IETF AIPREF            | 🚧     | Real successor to llms.txt/ai.txt. Drafts in WG Last Call. [datatracker](https://datatracker.ietf.org/group/aipref/about/) |
| UCP (Shopify + Google) | 🚧     | Universal Commerce Protocol, launched Jan 2026                                                                             |
| ACP (OpenAI + Stripe)  | 🚧     | Agentic Commerce Protocol                                                                                                  |
| AG-UI                  | 🚧     | Agent-to-frontend protocol from CopilotKit                                                                                 |

---

## Platform guides

How to actually ship these files on your stack:

- [WordPress](files/platforms/wordpress.md) — plugins (Yoast, Rank Math) or manual upload
- [Shopify](files/platforms/shopify.md) — workarounds for no root file access
- [Squarespace](files/platforms/squarespace.md) — code injection + hosted file workaround
- [Webflow](files/platforms/webflow.md) — Project Settings > Custom Code
- [Wix](files/platforms/wix.md) — premium plan workarounds
- [Next.js](files/platforms/nextjs.md) — `public/` directory or route handlers
- [Astro / Hugo / static](files/platforms/static.md) — drop in root
- [Custom server](files/platforms/custom.md) — middleware patterns

---

## Examples from real sites

See what working files actually look like in production:

- [`examples/llms-txt/`](examples/llms-txt/) — Anthropic, Stripe, Cloudflare, Perplexity, Vercel
- [`examples/agents-md/`](examples/agents-md/) — open source repos doing it well
- [`examples/robots-txt/`](examples/robots-txt/) — AI-aware configurations
- [`examples/tdmrep/`](examples/tdmrep/) — Elsevier, Springer Nature, IEEE

---

## Not on this list

These show up in other guides but don't belong here. See [`files/myths.md`](files/myths.md) for the full breakdown.

- `security.txt` — real, useful, not AI-related
- `<meta name="ai-content-url">` — no spec, no implementation
- `<meta name="llms">` — submitted to WHATWG, [closed as not planned](https://github.com/whatwg/html/issues/11548)
- `/.well-known/ai.txt` — multiple competing proposals, no adoption
- HTML comments for AI — most parsers strip them
- User-Agent sniffing — that's cloaking; use `Accept: text/markdown` instead

---

## Resources

- [llmstxt.org](https://llmstxt.org/) · [agents.md](https://agents.md/) · [a2a-protocol.org](https://a2a-protocol.org/) · [modelcontextprotocol.io](https://modelcontextprotocol.io/)
- [ai-robots-txt](https://github.com/ai-robots-txt/ai.robots.txt) — community AI bot list
- [GEO research paper](https://arxiv.org/abs/2311.09735) — Princeton/Georgia Tech/IIT Delhi/AI2

📖 **[Full guide with the why and context →](https://YOUR-BLOG-URL-HERE)**

---

## Contributing

PRs welcome. See [CONTRIBUTING.md](CONTRIBUTING.md). TL;DR: spec link required, status changes need a citation, vibes don't count.

## License

[CC0](LICENSE) — public domain. Fork it, copy it, ship it.
