# Prompt: `tdmrep.json`

**File:** `/.well-known/tdmrep.json` · **Status:** ✅ Legal standing in the EU · Honored indirectly by multiple AI companies · **Time to ship:** 15 to 30 min

The Text and Data Mining Reservation Protocol is a W3C specification that lets you formally declare whether your content can be mined for text, data, or AI training.

This is the one permission file with actual **legal teeth in the EU**. It implements the opt-out mechanism defined in Article 4 of the EU Copyright Directive (CDSM). Major publishers who take AI training seriously, including Elsevier, Springer Nature, IEEE, Sage, Radio France, and Le Parisien, all publish `tdmrep.json`. If you want a defensible, standards-based "no" that European AI companies are legally required to respect, this is it.

Spawning.ai's API also implements TDMRep on behalf of partners (Stability AI and others), so the file gets honored indirectly even by AI companies that don't read it themselves.

---

## Copy this prompt

```
Generate a tdmrep.json file for my website following the W3C Text and Data Mining
Reservation Protocol specification (https://w3c.github.io/tdm-reservation-protocol/spec/).

My site: [YOUR SITE URL]
My preference: [pick one]
  - Reserve all TDM rights (opt out of all text/data mining for AI training)
  - Allow TDM for research only (academic, non-commercial)
  - Allow TDM with compensation (commercial use requires a license; provide
    a tdm-policy URL pointing to my licensing page)
  - Mixed by path (e.g., /blog/* opt-out, /docs/* allowed)

If mixed by path, the paths and their settings: [LIST]

If I'm offering a licensing path, my tdm-policy URL: [URL OR "none"]

Required output:

1. The tdmrep.json file itself, as valid JSON conforming to the W3C spec:
   - Top-level "tdm" array of objects
   - Each object has "location" (path pattern), "tdm-reservation" (1 = rights
     reserved, 0 = not reserved), and optionally "tdm-policy" (URL to license)
   - Use "/" for the entire site, or specific paths if rules differ by area

2. Required HTTP response:
   - Served at /.well-known/tdmrep.json
   - Content-Type: application/json
   - Public, cacheable (24h+ is fine; this rarely changes)

3. Optional but recommended: an HTTP Link: header on every page pointing to
   the tdmrep.json (this is in the spec, and some validators check for it).

4. A brief plain-English explanation of what each field declares legally.

Return as code blocks I can copy directly.
```

## Heads up

- **EU-specific legal weight.** TDMRep implements Article 4 of the EU CDSM Directive. In the EU, AI companies training on opted-out content are violating copyright law. Outside the EU, enforcement is weaker, but the signal is still standards-based.
- **`tdm-reservation: 1` means "rights reserved" (opt-out).** Easy to get backwards. `0` means "not reserved" (allowed).
- **Pair with `ai.txt`.** Spawning.ai honors both. TDMRep gives you the legal standing; `ai.txt` gives you broader Spawning-network coverage.
- **Don't forget `/.well-known/`.** The file lives at `/.well-known/tdmrep.json`, not `/tdmrep.json`. Validators will reject it at the wrong path.
- **The Link header is part of the spec.** `Link: </.well-known/tdmrep.json>; rel="tdm-reservation"` on every response is what lets crawlers discover the policy without guessing the well-known path.
- **If you're offering a licensing path, the `tdm-policy` URL has to be real.** Don't point it at a 404; that's how you lose your defensible position in court.

## Verify it's working

```bash
# Confirm it's served at the right path with the right content-type
curl -sI https://yoursite.com/.well-known/tdmrep.json | head -5
# Expect: HTTP/2 200
#         content-type: application/json

# Validate JSON structure
curl -s https://yoursite.com/.well-known/tdmrep.json | jq .
# Expect: { "tdm": [ { "location": "/", "tdm-reservation": 1 }, ... ] }

# Check the Link header on a regular page
curl -sI https://yoursite.com/some-page | grep -i 'link:.*tdm'
# Expect: link: </.well-known/tdmrep.json>; rel="tdm-reservation"
```

Spawning.ai's [Have I Been Trained?](https://haveibeentrained.com/) tool also surfaces TDMRep status for sites in their network.

## See also

- [Spec: W3C TDM Reservation Protocol](https://w3c.github.io/tdm-reservation-protocol/spec/)
- [EU Copyright Directive Article 4](https://eur-lex.europa.eu/eli/dir/2019/790/oj): the legal basis
- [Real examples](../examples/tdmrep/): Elsevier, Springer Nature, IEEE
- [Prompt: `ai.txt`](ai-txt-spawning.md): pair for Spawning-network coverage
- [Prompt: AI meta tags](ai-meta-tags.md): per-page signaling layer
