# Myths and dead ends

Things that show up in other AI-files guides but don't belong on this list. Updated as the discourse changes.

## `security.txt` is not an AI file

[RFC 9116](https://www.rfc-editor.org/rfc/rfc9116). Lives at `/.well-known/security.txt`. Tells security researchers where to report vulnerabilities. **Add it for hygiene.** It's a real, useful file. Just not for AI.

## `<meta name="ai-content-url">`

No spec, no implementation, no origin. Cargo-culted in some blog posts. Don't add it.

## `<meta name="llms">`

Submitted to WHATWG as [issue #11548](https://github.com/whatwg/html/issues/11548). [Closed as "not planned."](https://github.com/whatwg/html/issues/11548#issuecomment-2393428537) The proposed alternative was an HTTP `Link` header, which is what the [markdown discovery prompt](../prompts/markdown-discovery.md) covers.

## `/.well-known/ai.txt`

Multiple competing proposals at this path, none with adoption. Not the same as Spawning's `/ai.txt` (which lives at root, not `/.well-known/`). If you want an AI permission file, use [Spawning's `ai.txt`](../prompts/ai-txt-spawning.md) or [`tdmrep.json`](../prompts/tdmrep-json.md).

## HTML comments for AI hints

```html
<!-- AI-READABLE-VERSION: /page.md -->
```

Most LLM HTML parsers strip comments before the model sees the content. Use a [hidden div](../prompts/ai-hint-div.md) instead.

## "Human / AI" toggle buttons

Agents don't click buttons. They fetch URLs. Provide the alternate URL via `<link>` tag, HTTP header, or content negotiation. See [markdown discovery](../prompts/markdown-discovery.md).

## Dedicated "AI info pages"

A page at `/ai` or `/for-ai` describing your site. No retrieval system treats these differently from regular pages. If you want to give AI a structured overview, that's what `llms.txt` is for.

## User-Agent sniffing to serve markdown

```
if (userAgent contains "GPTBot") { serve markdown } else { serve HTML }
```

This is cloaking. It violates Google's webmaster guidelines and can hurt your search rankings. Use `Accept: text/markdown` content negotiation instead: same URL, different representation, declared via `Vary: Accept`. See [markdown discovery](../prompts/markdown-discovery.md).

## "Submit your site to ChatGPT"

There's no submission endpoint. ChatGPT pulls from Bing's index (and OAI-SearchBot's crawl). To improve ChatGPT visibility:

1. Make sure Bing has indexed your site (Bing Webmaster Tools)
2. Make sure `OAI-SearchBot` isn't blocked in your `robots.txt`
3. Make sure key content is in HTML, not JavaScript-rendered

This applies similarly to Claude (uses Brave search), Gemini (Google), and Perplexity (its own crawler + multiple indexes).

## "Block all AI crawlers and you'll get more traffic"

Counter-intuitive but worth noting: blocking AI search crawlers (like `OAI-SearchBot`, not training crawlers like `GPTBot`) means you won't appear in AI answers. Many publishers have realized after the fact that they blocked themselves out of a real distribution channel. Consider blocking *training* bots while allowing *search* bots.

---

## Suggesting additions to this list

If you see another myth showing up in guides, open a PR. Include:

1. The claimed file or technique
2. Why it doesn't work (citation required: spec link, official statement, or empirical test)
3. The correct alternative if there is one
