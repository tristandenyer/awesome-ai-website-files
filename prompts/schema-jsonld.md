# Prompt: `schema.org` JSON-LD

**File:** `<script type="application/ld+json">` block in `<head>` · **Status:** ✅ Established for search and Copilot · 🚀 Foundation for emerging agentic standards · **Time to ship:** 5 min

Structured data in a JSON-LD script tag that describes what's on the page in a machine-readable vocabulary: this is an article, by this author, published on this date.

**Honest caveat:** a 2025 SearchVIU experiment found ChatGPT, Claude, Perplexity, and Gemini all _missed_ product data placed exclusively in JSON-LD. Only Microsoft Copilot (via Bing) reliably uses it for AI answers today.

**Why ship it anyway:** it's the foundation of [NLWeb](https://github.com/microsoft/NLWeb), the emerging standard for making websites queryable by AI agents. R.V. Guha (who invented RSS, RDF, and schema.org) is building the agentic web on top of schema.org at Microsoft. Good schema markup means you're already close to NLWeb-ready. It's also still the most established file on this list for traditional search (Google, Bing) and for Copilot.

---

## Copy this prompt

```
Generate schema.org JSON-LD for my web page.

Page URL: [YOUR PAGE URL]
Page type: [pick one]
  - Organization (homepage / about page)
  - Article (blog post / news)
  - Product (e-commerce listing)
  - LocalBusiness (storefront / service area)
  - Person (bio / portfolio)
  - FAQPage (FAQ section)
  - HowTo (step-by-step guide)
  - Event (date-bound event page)
  - WebSite (with SearchAction sitelink)

Content summary: [PASTE PAGE CONTENT OR DESCRIBE IT IN 2-3 SENTENCES]

Required:
- Use the official schema.org vocabulary (https://schema.org)
- Set "@context": "https://schema.org" and a single appropriate "@type"
- Include all required properties for the chosen @type
- Include recommended properties when the data is available
- Use ISO 8601 for dates (YYYY-MM-DD)
- Use absolute URLs, not relative
- For Organization/Person, include sameAs[] linking to social profiles if I provide them
- For Article, include author (Person or Organization), datePublished, dateModified, headline, image
- For Product, include offers with price and priceCurrency
- Validate against schema.org's required fields. Do not invent properties.

Output:
- A single <script type="application/ld+json"> block I can paste into <head>
- Pretty-printed JSON, no comments inside the JSON (JSON-LD spec disallows them)
- After the code block, a 2-3 line plain-English summary of what this markup tells AI/search engines
```

## Heads up

- **Don't expect AI answer engines to read this yet.** Today JSON-LD is mostly used by Google, Bing, and Copilot. ChatGPT/Claude/Perplexity won't pull product details from it reliably. Ship it for search + future-proofing, not as your AI visibility play.
- **The agentic-web bet.** schema.org is the substrate NLWeb is built on. If NLWeb adoption sticks, today's JSON-LD becomes tomorrow's `/ask` endpoint data source.
- **One @type per script block, but you can have multiple script blocks per page.** A blog post page commonly has both `Article` and `BreadcrumbList` in two separate blocks. Don't try to cram unrelated types into one graph unless you understand `@graph`.
- **Don't lie.** Google's [structured data policies](https://developers.google.com/search/docs/appearance/structured-data/sd-policy) forbid markup that doesn't match visible page content. Penalty risk is real.
- **`Article` vs `BlogPosting` vs `NewsArticle`.** Use `BlogPosting` for blog posts, `NewsArticle` only for journalism with an editorial process, `Article` as a generic fallback.
- **Images need absolute URLs and reasonable dimensions** (Google requires ≥1200px wide for `Article.image` to qualify for rich results).
- **`sameAs` is the AI signal.** Linking your Organization/Person to LinkedIn, GitHub, Wikipedia, Crunchbase, etc. is what lets AI engines disambiguate "which Tristan Denyer."

## Verify it's working

```bash
# Confirm the block is in the HTML
curl -s https://yoursite.com/page | grep -A 50 'application/ld+json'
```

Then validate with one of:

- [Google Rich Results Test](https://search.google.com/test/rich-results): checks eligibility for Google rich results
- [Schema.org Validator](https://validator.schema.org/): checks vocabulary correctness without Google's overlay
- [Google Search Console](https://search.google.com/search-console) → Enhancements: tracks valid/invalid items at scale once deployed

If you want to know whether AI engines are actually using your structured data, watch referrer traffic from `chat.openai.com`, `perplexity.ai`, and `claude.ai`. There's no public log of "an LLM read your JSON-LD," but referrals are the proxy.

## See also

- [Spec: schema.org](https://schema.org/)
- [Google's structured data docs](https://developers.google.com/search/docs/appearance/structured-data/intro-structured-data)
- [Article type reference](https://schema.org/Article)
- [Organization type reference](https://schema.org/Organization)
- [Real examples](../examples/): check production sites' page source for `application/ld+json`
