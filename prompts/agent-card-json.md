# Prompt: `agent-card.json` (A2A protocol)

**File:** `/.well-known/agent-card.json` · **Status:** 🚀 New, enterprise-led, 150+ companies adopted in 2025 · **Time to ship:** 30 min (if your agent already exists)

A JSON "business card" for an AI agent hosted at your domain. Part of Google's Agent2Agent (A2A) protocol, now governed by the Linux Foundation. Used so other agents can discover what your agent can do.

**Skip this unless you're running an agent.** This file is only relevant if you're _operating_ an AI agent (not just running a website). If you're building a customer-service agent, scheduling agent, or internal tool that should be callable by other AI systems (Salesforce, ServiceNow, SAP, and PayPal are all on A2A), this is how they find you. Most small sites can skip.

---

## Copy this prompt

```
I'm running an AI agent on my domain and want to make it discoverable to other
agents via the A2A (Agent2Agent) protocol. Generate a full agent-card.json
following the A2A spec (https://a2a-protocol.org/latest/topics/agent-discovery/).

My agent: [DESCRIBE in 2-3 sentences: what does it do, what problems does
  it solve, who's the intended caller?]
Agent endpoint URL: [where the agent service is hosted, e.g., https://agent.mysite.com]
Agent version: [e.g., 1.0.0]
Provider/owner: [your org name + URL]

What the agent can do (list 2-5 "skills"):
  - [skill name]: [one-line description]
  - [skill name]: [one-line description]
  - ...

For each skill, include input/output schema where you can. JSON Schema preferred.

Operational details:
  - Streaming responses supported? [yes/no]
  - Authentication required? [none / Bearer token / OAuth2 / API key / mTLS]
  - Default input format(s): [e.g., text/plain, application/json]
  - Default output format(s): [e.g., application/json]
  - Rate limits the caller should know about: [DESCRIBE OR "none"]

Required output:

1. The agent-card.json file content, as valid JSON conforming to the A2A spec.

2. Served at /.well-known/agent-card.json with:
   - Content-Type: application/json
   - 200 status
   - Cacheable (1 hour is reasonable for stable agents)

3. A short callout for any required fields I left ambiguous or that need
   verification against the live spec.

4. Brief plain-English summary of what an A2A-compatible client would see
   when they read this card.
```

## Heads up

- **You actually need an agent.** Don't ship `agent-card.json` for a static website. The card describes a callable service; pointing it at a 404 or a marketing page hurts your reputation in the agent ecosystem.
- **Skill descriptions are the searchable surface.** Calling agents will route on these. "Answers questions" is useless; "Looks up order status from order ID and email" is useful.
- **Auth is non-trivial.** A2A supports several auth modes. Pick one consistent with your agent's actual security posture; don't ship `auth: none` if your agent can mutate state.
- **Versioning matters.** When you change skills, bump `agent.version` so callers can detect compatibility breaks.
- **Linux Foundation governance.** Spec is now community-governed; expect minor schema evolution. Pin to a spec version in your card.
- **Pair with [NLWeb](nlweb.md) thoughtfully.** NLWeb is for site content discovery; A2A is for agent-to-agent calls. They're complementary, not redundant.

## Verify it's working

```bash
# Card is at the well-known path
curl -sI https://yoursite.com/.well-known/agent-card.json | head -5
# Expect: HTTP/2 200
#         content-type: application/json

# Card is valid JSON and has expected fields
curl -s https://yoursite.com/.well-known/agent-card.json | jq '.name, .version, .skills | length'

# Endpoint declared in the card actually responds
ENDPOINT=$(curl -s https://yoursite.com/.well-known/agent-card.json | jq -r '.endpoint // .url')
curl -sI "$ENDPOINT" | head -3
```

A2A also publishes a [conformance test suite](https://a2a-protocol.org/); run it before claiming compatibility.

## See also

- [Spec: A2A Protocol](https://a2a-protocol.org/)
- [A2A on GitHub](https://github.com/a2aproject)
- [Prompt: `mcp.json`](mcp-json.md): adjacent agent-discovery file with different ecosystem
- [Prompt: NLWeb](nlweb.md): auto-publishes as MCP, complements A2A
