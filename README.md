# PawMultiverse open data — pet food recalls, five official registers

A nightly mirror of the open datasets published by [pawmultiverse.com](https://pawmultiverse.com/),
a record of pet food recalls built by reading five official government registers on a
schedule and **keeping every entry after the authority takes it down**.

Everything here is CC BY 4.0. Take it, use it, name the source.

## What is in the record

| Register | Country | Publisher |
| --- | --- | --- |
| RappelConso, animal entries | FR | DGCCRF |
| Animal and veterinary recalls | US | FDA |
| Pet food recall enforcement reports | US | FDA |
| Pet food alerts | GB | Food Standards Agency |
| Recalls and safety alerts, pet entries | CA | Government of Canada |

Live totals, and when each register was last read without error, are in
[`data/status.json`](data/status.json). At the time this repository was created the record
held 210 entries.

## The part that exists nowhere else

Each authority publishes only its own country. This record matches products **across**
those registers on barcode, so it can show the same product recalled in more than one
country — see [`data/cross-border.json`](data/cross-border.json).

It also keeps every dated version of a notice, so a change an authority made to its own
published wording after announcing it stays visible. That is what
[`data/changes.json`](data/changes.json) holds.

## The files

| File | What it is |
| --- | --- |
| `data/records.json` | Every entry held, newest first, with source, country, status and last-seen date |
| `data/records.csv` | The same list flattened for spreadsheets |
| `data/cross-border.json` | Products matched across two or more country registers on barcode |
| `data/changes.json` | Dated versions — what an authority changed after publishing |
| `data/sources.json` | The registers, their publishers and their licences |
| `data/status.json` | Totals and the last successful read per register |
| `data/openapi.json` | OpenAPI 3.1 description of the live API |
| `data/LAST_MIRRORED.txt` | When this mirror last ran |

## How the mirror works

[`.github/workflows/mirror.yml`](.github/workflows/mirror.yml) runs nightly at 04:20 UTC. It
downloads everything into a staging folder, checks each file parses and that the record is
not empty, and only then copies it into `data/`. It commits only when something actually
changed. **Partial or empty data is never committed**, so a bad night leaves the previous
mirror standing rather than replacing it with nothing.

## Reading it live instead

The mirror is a convenience. The record itself is live:

- API — `https://pawmultiverse.com/wp-json/mv/v1/`, described in [openapi.json](https://pawmultiverse.com/openapi.json)
- Check a barcode across all five registers — `/wp-json/mv/v1/bag?code=`
- Model Context Protocol server for assistants — [/mcp-recalls](https://pawmultiverse.com/mcp-recalls)
- How the record is built and checked — [the method page](https://pawmultiverse.com/pet-food-recalls/method/)

## Two things to be careful about

**A barcode with no match is not a statement that a product is safe.** It means no listed
authority has published a recall carrying that barcode in what has been read so far. Many
recalls are announced by lot code or best-before date rather than by barcode, so a product
can be affected and still not match. Every tool built on this data should say so.

**This is a record of what authorities published, not veterinary advice.** It holds no
dosing, toxicity or treatment information. If an animal is unwell, the answer is a
veterinarian.

## Licence and attribution

Data: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — see
[LICENSE-DATA.md](LICENSE-DATA.md).

> Paw Multiverse, pawmultiverse.com, CC BY 4.0

The underlying register entries remain the work of the publishing authorities, under their
own terms, which are recorded per source in `data/sources.json`.

Published by A.I.T. Multiverse Consulting Ltd, Nicosia, Cyprus.
