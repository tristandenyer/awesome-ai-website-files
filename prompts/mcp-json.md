# Prompt: `mcp.json` (MCP server manifest)

**File:** `/.well-known/mcp.json` (+ a real MCP server running somewhere) · **Status:** 🚀 Fast-growing, backed by Anthropic, OpenAI, Google, Microsoft · **Time to ship:** 4 to 16 hr

A manifest declaring that your domain hosts a Model Context Protocol server. MCP is the "USB-C for AI applications" built by Anthropic and now adopted across OpenAI, Google, and Microsoft.

If you expose tools or data that AI assistants should be able to use (a product catalog, internal docs search, ability to take actions on behalf of users), MCP is how you describe them once and have every major AI client connect with no per-platform work. SDK download volumes are reported in the tens of millions per month.

**Skip this unless you have something to expose.** This file is paired with a real running MCP server. Don't publish it for a marketing site.

---

## Copy this prompt

```
I want to expose an MCP (Model Context Protocol) server for my site so AI
assistants like Claude, ChatGPT, Gemini, and Copilot can interact with it.

What I'd like to expose:
  [DESCRIBE, e.g., "product catalog and order status", "internal documentation
   search", "ability to draft and send transactional emails", "calendar
   scheduling"]

My users / callers: [DESCRIBE: internal employees? customers? both?]

Sensitivity of the data/actions: [pick]
  - Read-only public data (low sensitivity)
  - Read-only authenticated data (medium)
  - Writes / mutations / billing actions (high)

My stack: [DESCRIBE language and runtime preference, e.g., "TypeScript on
  Cloudflare Workers", "Python on AWS Lambda", "Go on my own VPS"]

Help me with:

1. Honest go/no-go: is MCP the right fit for what I want to expose? (MCP shines
   for AI-facing tool use; not every integration belongs in MCP.)

2. If yes, a minimal plan:
   - Which MCP SDK to use (Python, TypeScript, or another)
   - A skeleton server exposing 2-3 example tools for my use case, with
     proper JSON Schema for inputs
   - Authentication approach matched to the sensitivity I described
   - Where to host it (HTTP transport details, not stdio)
   - How to expose it as /.well-known/mcp.json on my domain

3. A sample /.well-known/mcp.json manifest pointing at my server, with the
   correct fields per the latest spec at https://modelcontextprotocol.io/.

4. How to test locally with Claude Desktop or the MCP inspector before
   publishing.

5. Logging and observability: what to capture so I can tell who's calling
   what tool and how often.

Assume I can write code but have never built an MCP server.
```

## Heads up

- **MCP is for tool use, not content discovery.** If you just want AI to read your content, [`llms.txt`](llms-txt.md) + [`.md` routes](md-routes.md) is lighter and more appropriate. MCP shines when you have _actions_ or _live queries_ to expose.
- **Auth is mandatory for anything sensitive.** "It's behind `/.well-known/mcp.json`" is not security. Use OAuth, bearer tokens, or signed requests for any tool that can mutate state or read non-public data.
- **HTTP transport, not stdio.** For a public/web-facing MCP server, you want the HTTP transport. stdio MCP is for local tools (e.g., Claude Desktop running a CLI).
- **NLWeb auto-publishes as MCP.** If you're already shipping [NLWeb](nlweb.md), you may not need a separate `mcp.json`; NLWeb advertises itself as an MCP server. Add `mcp.json` only if you have tools beyond NLWeb's content-query surface.
- **Spec is moving.** MCP is < 2 years old as of 2026 and evolving. Pin a spec version in your manifest and watch the modelcontextprotocol.io changelog.
- **Don't expose dangerous tools.** "Run shell command," "send arbitrary email," "transfer money": if it would be unsafe to put behind a bot, it's unsafe over MCP. Apply the same threat model.

## Verify it's working

```bash
# Manifest is at the well-known path
curl -sI https://yoursite.com/.well-known/mcp.json | head -5
# Expect: HTTP/2 200
#         content-type: application/json

# Manifest validates and points at a live server
curl -s https://yoursite.com/.well-known/mcp.json | jq '.server, .tools | length'

# Server responds to MCP handshake (exact command depends on transport)
# For HTTP transport, use the MCP inspector:
npx @modelcontextprotocol/inspector https://yoursite.com/mcp
```

For deeper validation, connect via Claude Desktop's MCP config and confirm your tools appear in the tool picker.

## See also

- [Spec: Model Context Protocol](https://modelcontextprotocol.io/)
- [MCP SDKs (TypeScript, Python, etc.)](https://github.com/modelcontextprotocol)
- [MCP Inspector](https://github.com/modelcontextprotocol/inspector): local validation tool
- [Prompt: NLWeb](nlweb.md): auto-publishes as MCP, may make this redundant
- [Prompt: `agent-card.json`](agent-card-json.md): A2A-protocol counterpart
