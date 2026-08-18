# Prompt: `api-catalog`

**File:** `/.well-known/api-catalog` · **Status:** ✅ IETF Standards Track ([RFC 9727](https://www.rfc-editor.org/rfc/rfc9727.html), June 2025) · **Time to ship:** 20 min

If your domain publishes APIs, RFC 9727 gives machine clients — including AI agents — one standard place to discover all of them. A GET to `/.well-known/api-catalog` returns a linkset document ([RFC 9264](https://www.rfc-editor.org/rfc/rfc9264.html), `application/linkset+json`) listing every API you publish, so an agent doesn't have to scrape your docs site to learn what you offer.

This pairs naturally with the rest of your API-facing stack: OpenAPI describes each API, `mcp.json` points at your MCP servers, and `api-catalog` is the index that ties it together at a standards-track address.

Skip this one if your site doesn't publish APIs — it's for API providers only.

---

## Copy this prompt

```
Generate an RFC 9727 api-catalog document for my domain.

My site: [YOUR SITE URL]
My APIs: [LIST EACH API'S NAME + DOCS OR SPEC URL, e.g.
  - Orders API — https://developer.example.com/apis/orders (OpenAPI at /orders/openapi.json)
  - Search API — https://developer.example.com/apis/search]

Required output:

1. The api-catalog document as valid application/linkset+json per RFC 9264:
   - Top-level "linkset" array
   - One object with "anchor" set to https://[MY DOMAIN]/.well-known/api-catalog
   - An "item" array with one {"href": ...} per API, pointing at a page or
     document where a machine client can learn the API's purpose, usage
     policy, and endpoint (OpenAPI documents are ideal targets)

2. Serving requirements:
   - Served at /.well-known/api-catalog over HTTPS
   - Content-Type: application/linkset+json;
       profile="https://www.rfc-editor.org/info/rfc9727"
   - HEAD requests should include a Link header with rel="api-catalog"

3. Platform-specific deployment notes for: [Next.js / nginx / Apache /
   Cloudflare Workers — PICK YOURS], including how to set the exact
   content type for an extensionless well-known path.

4. A curl command to verify the deployed document parses as JSON and
   contains my "linkset" array.
```

---

## Verify it worked

```bash
curl -s -H "Accept: application/linkset+json" \
  https://yoursite.com/.well-known/api-catalog | python3 -m json.tool
```

You should see your `linkset` array. Then run the [AI Readiness Check](https://www.tristandenyer.com/ai-readiness-check) — the api-catalog card validates the linkset structure, not just the file's presence.

## Spec and further reading

- [RFC 9727 — api-catalog well-known URI](https://www.rfc-editor.org/rfc/rfc9727.html)
- [RFC 9264 — Linkset](https://www.rfc-editor.org/rfc/rfc9264.html)
- Related: [`mcp.json`](mcp-json.md) for MCP server discovery on the same domain
