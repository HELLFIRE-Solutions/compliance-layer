# HELLFIRE Internal Compliance Standard v1

Status: Etap 1 — internal standard, documented 2026-07-21. Not yet a client-facing
audit product (that's Etap 2, see root [README.md](../README.md)).

## Scope and purpose

DSGVO/AI Act compliance is HELLFIRE's entry ticket to the German market (see
`project-overview` memory / `STATE.md`) and a differentiator against competitors
who bolt compliance on late. Etap 1's job is narrow: **write down how HELLFIRE
already handles compliance internally**, so every module built from here on
points at one standard instead of each session inventing its own rules.

This is not new policy invented from scratch. It codifies decisions already made
and running in code (gtm-agent's `LegitimateInterestRecord`) and methodology
already proven in the sibling TETA+PI project (TWIRA's evidence-tiered,
auditable trust classification). Where this document states a rule, it's
because a real module already needed the answer — not because compliance-layer
pre-empted a decision that belonged to the module building it.

## The three components

1. **[Legitimate interest documentation](legitimate-interest.md)** — the
   DSGVO Art. 6(1)(f) standard for any module that contacts a natural person
   without prior consent. Adopts gtm-agent's `LegitimateInterestRecord` schema
   as-is; does not introduce a second standard.
2. **[Data residency](data-residency.md)** — where HELLFIRE's infrastructure and
   its modules' data may live, and the current gaps against that rule.
3. **[AI Act risk classification](ai-act-classification.md)** — a working risk
   tier for each of HELLFIRE's own AI systems (the module catalog), and the
   trigger conditions that would force a re-classification.

## Coordination record

Per this module's kickoff prompt: coordinate with gtm-agent (session 05) on how
it documents legitimate interest, rather than inventing a second standard.
Read `gtm-agent/docs/architecture.md` §3 and `gtm-agent/src/gtm_agent/models.py`
before writing anything here. Result: gtm-agent's `LegitimateInterestRecord` is
adopted unchanged as the HELLFIRE-wide schema (see
[legitimate-interest.md](legitimate-interest.md)) — compliance-layer's Etap 1
contribution is naming it as the standard other modules must reuse, and adding
the parts gtm-agent explicitly deferred (audit/export view, periodic
re-validation tracking), not changing the capture schema itself.

## Relationship to TETA+PI / TWIRA

TWIRA (`TETA+PI/docs/overview.md`, `docs/glossary.md`) is TetaPi GmbH's
query-time ranking algorithm — `α·T + β·I + γ·P`, Trust × Intent × Provenance —
and is not itself a compliance tool. What Etap 1 borrows from it is the
**methodology**, not the algorithm:

- Trust/verification is a tiered classification (`none | registry | partial |
  full | live`) driven by concrete evidence, not a binary yes/no.
- Every classification event is append-only and timestamped (TWIRA's
  `verification_events` "Temporal Moat") — a classification has a
  `first_verified_at` and can be re-checked, but its history can't be quietly
  rewritten.
- Classification decays/needs re-validation over time rather than being
  assumed permanent.

[ai-act-classification.md](ai-act-classification.md) applies this same shape —
tiered, evidence-based, re-validated over time, with an audit trail — to AI Act
risk classification instead of trust ranking. gtm-agent's
`LegitimateInterestRecord.review_date` field already applies the same
"classification decays" principle independently; this is the second module to
converge on it, which is a signal it belongs in a shared pattern rather than
being redecided per module.

## What Etap 1 deliberately does not do

- No audit checklist/questionnaire product yet — that's Etap 2.
- No changes to gtm-agent's code or schema.
- No legal sign-off — these documents are HELLFIRE's internal working
  interpretation, written by a non-lawyer session, not legal advice. Flagged
  open items in [data-residency.md](data-residency.md) and
  [ai-act-classification.md](ai-act-classification.md) should go to actual
  counsel before Etap 2 turns this into a client-facing product.
