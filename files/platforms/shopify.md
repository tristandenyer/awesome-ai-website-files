# Shopify

Shopify's biggest constraint: **you can't put files at the site root.** Workarounds exist for every file on the list.

## The root-access problem

Shopify hosts your store on their CDN. You don't control the file system, so you can't drop `llms.txt` directly at the root. Three workarounds:

1. **Apps from the Shopify App Store** (easiest)
2. **Liquid template + URL redirect** (no app needed)
3. **External hosting + redirect** (most flexible)

## `robots.txt`

Shopify generates a default `robots.txt`. To customize:

- Online Store → Themes → Edit code → create `robots.txt.liquid`
- Add your AI bot rules using Liquid syntax
- Shopify [official docs](https://shopify.dev/docs/storefronts/themes/seo/robots-txt) cover the format

Use the [`robots-txt` prompt](../../prompts/robots-txt.md) to generate the rules, then translate to Liquid.

## `llms.txt`

**Option 1: App** (recommended)

- "llms.txt Generator AI Search" or similar in the Shopify App Store
- Auto-updates as you add products and collections
- App handles hosting and the root-URL redirect

**Option 2: Liquid template**

1. Create a template `templates/page.llms.liquid`
2. Set Content-Type to plain text in the template
3. Create a Page with handle `llms` assigned to that template
4. Now create a redirect: Online Store → Navigation → URL Redirects
5. Redirect from `/llms.txt` → `/pages/llms`

**Option 3: External file + redirect**

1. Host the file somewhere you control (GitHub Pages, Cloudflare R2, Firebase, etc.)
2. Online Store → Navigation → URL Redirects
3. Redirect from `/llms.txt` to the external URL

The redirect approach works but adds a hop, and some AI tools may not follow it. Liquid template is more reliable.

## `llms-full.txt`

Same workarounds as `llms.txt`. For ecommerce specifically, this is high-value: you're concatenating product descriptions, policies, and key pages into one file an AI can ingest.

## `.md` routes

Not natively supported. Workarounds are awkward, so most Shopify stores skip this and rely on the JSON-LD that Shopify already outputs in HTML.

## `schema.org` JSON-LD

Shopify outputs schema.org JSON-LD by default for products, collections, and articles. To extend:

- Edit `theme.liquid` and add custom `<script type="application/ld+json">` blocks
- Or use an app like "JSON-LD for SEO"

## `ai.txt` (Spawning)

Same root-access problem. Use the same Liquid template + redirect pattern as `llms.txt`.

## `tdmrep.json`

Path is `/.well-known/tdmrep.json`, which needs the same external-hosting + redirect workaround. Shopify doesn't natively support `.well-known/` paths.

## NLWeb / MCP / agent files

These require running services, not just static files. You'd host the service externally (your own server, Vercel, Cloudflare Workers) and point AI tools to that endpoint. Not a Shopify-specific workflow.

## Heads up

- **Theme updates can clobber custom files.** Document what you've added so you can re-apply after theme switches.
- **App approach is safer for non-technical merchants.** Apps update with platform changes.
- **Test in incognito.** Shopify aggressively caches, and you may see stale content for hours.

## Verification

```bash
curl -I https://yourstore.myshopify.com/llms.txt
# Should return 200 (or 301 → 200 if using redirect approach)
```

If you see a 301 to a CDN URL, the redirect works but adds a hop. AI tools usually follow redirects, but not always.

## Community resources

- [Adding LLMs.txt to Shopify](https://community.shopify.dev/t/adding-llms-txt-file-to-shopify-store/19276): Shopify Developer Community thread with current workarounds
