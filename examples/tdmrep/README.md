# `tdmrep.json` examples

Real-world `tdmrep.json` files from publishers using the W3C Text and Data Mining Reservation Protocol.

## Status

🚧 This folder is being seeded. PRs welcome.

## Public adopters worth examining

Major publishers known to publish `tdmrep.json`:

- **Elsevier:** typically at `/.well-known/tdmrep.json` on their journal domains
- **Springer Nature:** across their academic publishing properties
- **IEEE:** engineering and technical publications
- **Sage Publishing:** academic journals
- **Radio France:** French public broadcaster
- **Le Parisien:** French newspaper

To check any site for a `tdmrep.json`:

```bash
curl https://example.com/.well-known/tdmrep.json
```

## Why this matters

`tdmrep.json` is the only AI permission file with **legal teeth in the EU**. It implements the opt-out mechanism defined in Article 4 of the EU Copyright Directive (CDSM). EU-based AI companies are legally required to respect a properly-formed reservation.

For a defensible, standards-based "no" to AI training, this is the file to publish. It's also honored indirectly through Spawning's API, which is integrated by Stability AI and others.

## Adding examples

PRs welcome. Submit:

1. Real production `tdmrep.json` from a publisher (with source URL)
2. A header comment showing where it was fetched from
3. A `# Why this is a notable example` section explaining the pattern

See [CONTRIBUTING.md](../../CONTRIBUTING.md).

## Spec

[W3C TDM Reservation Protocol](https://w3c.github.io/tdm-reservation-protocol/spec/)
