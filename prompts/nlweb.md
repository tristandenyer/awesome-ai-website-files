# Prompt: NLWeb

**File:** A live `/ask` endpoint on your domain (running the NLWeb service) · **Status:** 🚀 New, Microsoft-backed, schema.org-based · **Time to ship:** 4 to 16 hr depending on data shape

NLWeb is an open-source project from Microsoft that turns your website into a natural language interface. Every NLWeb instance automatically becomes a Model Context Protocol (MCP) server, which means any AI assistant (ChatGPT, Claude, Gemini, Copilot) can query your site's content directly.

It's designed by R.V. Guha (creator of schema.org, RSS, and RDF) and explicitly framed as "HTML for the agentic web." Instead of an AI agent navigating Tripadvisor's search filters, it can just ask: "Find family-friendly restaurants in Barcelona with outdoor seating." The response comes back as structured schema.org JSON.

Early adopters: Shopify, Tripadvisor, Eventbrite, O'Reilly Media, Hearst. If your site already has good [`schema.org` JSON-LD](schema-jsonld.md), you're halfway there.

Unlike most files in this repo, NLWeb is a **service** you run, not a static file you upload. It's open source, and you self-host the endpoint.

---

## Copy this prompt

```
I want to make my website queryable by AI agents using Microsoft's NLWeb standard
(https://github.com/microsoft/NLWeb).

My site: [YOUR SITE URL]
What the site does: [DESCRIBE in 1-3 sentences]

Data I already publish in structured form (pick any that apply):
  - schema.org JSON-LD on pages
  - RSS or Atom feed
  - Product catalog API
  - Sitemap
  - A database I could export
  - CMS with structured fields (WordPress custom post types, Sanity, etc.)
  - "I don't know"; tell me how to check

The kinds of questions I'd want agents to answer about my content:
  [LIST 3-5 EXAMPLE QUESTIONS, e.g., "what are the latest articles about X",
   "find products under $50 with feature Y", "events near zip code Z next month"]

My deployment constraints: [pick]
  - I have my own server / VPS / can run Docker
  - I'm on a managed PaaS (Vercel, Netlify, Cloudflare Workers)
  - I'm on shared hosting / WordPress.com / Squarespace (limited)
  - I can run things on a separate subdomain (api.mysite.com)

Help me with:

1. Honest assessment: is NLWeb worth shipping for my site today? Be candid.
   For a 10-page marketing site, probably not. For a content-heavy or
   product-heavy site with existing structured data, probably yes.

2. If yes, the minimum viable setup:
   - Where my structured data should come from (schema.org on pages? a feed?
     a direct DB export?)
   - Which NLWeb deployment recipe fits my constraints
   - The /ask endpoint URL pattern and how to wire it up to my domain
   - Authentication and rate limiting: what's needed at minimum

3. How NLWeb's automatic MCP-server behavior works, and what AI assistants
   will see when they discover my endpoint.

4. A test plan: a sample natural-language query I can run against my endpoint
   to verify it returns valid schema.org JSON.

Assume I can deploy code but have never built a query/inference service.
```

## Heads up

- **Schema-first.** NLWeb works by translating natural language into queries against your structured data. If your structured data is bad or absent, NLWeb output will be bad. [Ship `schema.org` JSON-LD first](schema-jsonld.md).
- **It's a service, not a file.** You can't `curl` an NLWeb installation into existence. Plan on hosting + an LLM API key (OpenAI, Anthropic, or Azure OpenAI) for the natural-language layer.
- **Costs are real.** Every `/ask` query costs LLM tokens. Rate-limit aggressively before exposing to the public internet.
- **Auto-MCP is the killer feature.** Once NLWeb is running, AI assistants don't need a separate `/.well-known/mcp.json`; NLWeb advertises itself as MCP. If you ship NLWeb, you can probably skip [`mcp.json`](mcp-json.md) unless you have additional non-NLWeb tools to expose.
- **Backed by Microsoft, but open-source.** No lock-in. The reference implementation is on GitHub; you can self-host or use any hosting partner.

## Verify it's working

```bash
# Endpoint should be live
curl -sI https://yoursite.com/ask | head -5

# Sample query (POST varies by NLWeb deployment; check the spec)
curl -s -X POST https://yoursite.com/ask \
  -H "Content-Type: application/json" \
  -d '{"query":"YOUR NATURAL-LANGUAGE QUERY"}'
# Expect: JSON response with schema.org @type'd objects
```

For deeper validation, point [Claude Desktop](https://claude.ai/download) or another MCP client at your endpoint and see if it appears as an available tool.

## See also

- [Microsoft/NLWeb on GitHub](https://github.com/microsoft/NLWeb)
- [Prompt: `schema.org` JSON-LD](schema-jsonld.md): prerequisite
- [Prompt: `mcp.json`](mcp-json.md): alternative if you don't want full NLWeb
- [Prompt: `agent-card.json`](agent-card-json.md): A2A-protocol companion
