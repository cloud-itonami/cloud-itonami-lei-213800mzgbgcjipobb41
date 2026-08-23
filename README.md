# cloud-itonami-lei-213800mzgbgcjipobb41

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by YouGov plc.**

This repository archives the publicly published Terms of Use / Terms and Conditions of
**YouGov plc**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: YouGov plc — the live GLEIF record spells it `YOUGOV PLC` (language `en`,
  last updated 2026-02-10) and lists no other or previous names (`otherNames` is empty).
- **LEI (ISO 17442)**: [213800MZGBGCJIPOBB41](https://search.gleif.org/#/record/213800MZGBGCJIPOBB41) (GLEIF-verified)
- **Jurisdiction**: GB — confirmed against the live GLEIF record: registered with Companies
  House (`RA000585`, England and Wales, company number `03607311`, ISO 20275 legal form
  `B6ES` Public Limited Company), created 1998-07-30; legal address and headquarters
  address are the same, 50 Featherstone Street, London EC1Y 8RT (`GB-LND`). `facts.edn`
  below carries the registry's answer with provenance.
- **Website**: https://yougov.com
- **Ticker**: YOU (LSE) — a listing named here from discovery context, not read from
  GLEIF. GLEIF maps **1 ISIN** to this LEI (`GB00B1VQ6H25`, mirrored in `facts.edn`);
  which market it trades on is not something GLEIF answers, so nothing beyond the
  identifier is asserted.

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived Terms of Use documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 14 verified registry facts with per-fact provenance (the entity, its
  securities count and the 1 identifier behind it, issuer and issuer accreditation,
  registration authority, legal form, both parent-reporting exceptions, the
  direct-children count and the 4 children behind it). **Generated** — see below.
- `scripts/verify-facts.cljs` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind them.
`facts.edn` now carries them as data, and every value in it was read out of a public
registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljs           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljs --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO requests back the file (`CHECKED 11` when it was written,
2026-08-23T10:27Z, golden copy 2026-08-23T00:00Z) — the LEI record (legal name
`YOUGOV PLC`, jurisdiction `GB`, entity category `GENERAL`, entity **ACTIVE**,
registration **ISSUED** since 2017-04-11 with the next renewal due 2027-04-11, last
updated 2026-02-10, `FULLY_CORROBORATED`, conformity flag `CONFORMING`, no BIC,
OpenCorporates id `gb/03607311`, S&P Global id `20726037`; entity status and
registration status are different fields and are recorded separately), its **1 ISIN** as
a count read from `meta.pagination.total` of the cited page plus one `:security` entity
for the identifier, its managing LOU and LEI-issuer accreditation (London Stock Exchange
LEI Limited, LEI `213800WAVVOPS85N2205`, accredited 2017-11-06), registration authority
`RA000585` (Companies House, England and Wales), ISO 20275 legal form `B6ES` (`Public
Limited Company`, `GB`), reporting exceptions at both consolidation levels
(`NON_CONSOLIDATING` — GLEIF's reason code for an entity that does not consolidate
under any parent at that level, so the registry names no parent at either level; this
file records that answer and nothing about who owns this entity is asserted here), and
a measured **4 direct children**, read from `meta.pagination.total` of the cited page
(one page of 15), each mirrored as a `:direct-child` entity: YouGov Netherlands B.V.
(`NL`), YouGov M.E. FZ-LLC (`AE-DU`), YG Research India Private Limited (`IN`) and
YouGov Nordic & Baltic A/S (`DK`), all `ACTIVE`, all `IS_DIRECTLY_CONSOLIDATED_BY` this
entity. That is the registry's list of entities that report this LEI as their direct
accounting-consolidation parent; it is not a group chart, and a subsidiary that holds no
LEI or reports an exception does not appear in it — so absence from this list is not
evidence that a subsidiary does not exist. The
`direct-parent` and `ultimate-parent` endpoints answered `404` because GLEIF publishes
the exception side of that pair for this entity, which the checker treats as a fact
rather than a failure.

The checker's exit codes are three, not two: `0` every recorded fact matches the live
sources, `1` a citation broke or a fact drifted, `3` the check could not be performed at
all — an absent `facts.edn`, or every request failing at the transport level. A check
that could not run must not be indistinguishable from a check that ran and found
nothing, so it refuses to report a pass rather than exiting 0. All outcomes were
exercised before this landed: unmodified `0` (`OK all 14 recorded fact(s) still match`);
`:company/jurisdiction` rewritten to `JE` → `1` naming `DRIFT gleif-lei-record
:company/jurisdiction`; `:securities/isin-count` edited `1` → `2` → `1` naming
`DRIFT gleif-isins :securities/isin-count`; the `:security` entity's ISIN rewritten →
`1` naming `DRIFT gleif-isin-gb00b1vq6h25 :securities/isin`; the measured
`:relationship/direct-child-count` rewritten `4` → `3` → `1` naming
`DRIFT gleif-direct-children-count`; the YouGov Netherlands `:direct-child` entity
deleted → `1` naming it `ADDED` (the live registry still lists it); both levels'
`:relationship/exception-reason` rewritten to `NO_KNOWN_PERSON` → `1` naming the drift
in both `gleif-direct-parent-reporting-exception` and
`gleif-ultimate-parent-reporting-exception`; company number `03607311` rewritten
`03607312` → `1` naming the drift in `gleif-lei-record` and
`gleif-registration-authority`; `:elf/local-name` rewritten → `1` naming
`DRIFT iso-20275-entity-legal-form :elf/local-name`; `blueprint.edn`'s `:company/lei`
edited → `1` (`facts.edn records a different :company/lei than blueprint.edn`); the
GLEIF host in the checker rewritten to an unresolvable name → `3` (`INCONCLUSIVE could
not reach GLEIF at all … refusing to report a pass`); and with no `facts.edn` at all →
`3` (`INCONCLUSIVE facts.edn is missing or holds no facts`). Each mutation was reverted
and the restored file compared byte-for-byte against the generated one. One attempted
mutation did not take (a GNU-only `sed` address on macOS left the file untouched) and
the run came back `0`; a byte comparison of the file before running caught it, and the
mutation was redone — a green result on a file that was not actually broken is the
oldest way a gate turns into theatre.

`facts.edn` is not yet on the shared query plane: `manifest/edn-query.cljs` in
`com-junkawasaki/root` has loaders for `blueprint.edn` and the ToS journal and none for
this file, so its datoms load here but are not joinable from `edn-query`.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
