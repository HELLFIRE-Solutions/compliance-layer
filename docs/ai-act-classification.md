# AI Act Risk Classification (Working Standard)

Status: internal working classification, 2026-07-21. **Not legal advice** —
this is a non-lawyer's working interpretation of Regulation (EU) 2024/1689,
meant to catch obviously-wrong assumptions early and flag genuinely close
calls for real review before Etap 2. Verify current obligation dates and
Annex III scope against the official text before this leaves internal use.

## Method (borrowed from TWIRA, applied to a different question)

TETA+PI's TWIRA is a trust-ranking algorithm, not a compliance tool — see
[standard.md](standard.md) for that distinction. What this document borrows is
the *shape* of TWIRA's approach to classification, because it already solved
a structurally similar problem (classify an entity along a tier, based on
evidence, re-checked over time, with a record of when the classification was
made):

- **Tiered, not binary.** TWIRA uses `none | registry | partial | full | live`.
  AI Act risk uses its own four tiers (below) — reused *as a shape*, not
  literally the same tiers.
- **Evidence-based, not self-declared.** A module doesn't get to assert its
  own risk tier; the classification here is justified against what the module
  actually does (who it contacts, what decisions it makes about a person,
  whether a human is in the loop before anything external happens).
- **Re-validated, not permanent.** A module's risk tier can change if its
  scope changes (see "Re-classification triggers" below) — same principle as
  gtm-agent's `LegitimateInterestRecord.review_date` and TWIRA's decay/re-check
  behavior. This table should be revisited whenever a module's Etap 2 (or
  later) scope changes materially, not just once at Etap 1.

## The four tiers (EU AI Act)

1. **Unacceptable risk (Art. 5)** — banned outright (social scoring,
   manipulative/subliminal techniques, most biometric categorization, etc.)
2. **High-risk (Annex III)** — specific listed categories (employment/hiring
   decisions, credit scoring, law enforcement, critical infrastructure, etc.)
   — heavy obligations (risk management, human oversight, documentation,
   conformity assessment)
3. **Limited risk (Art. 50)** — transparency obligations: disclose AI
   involvement when a person interacts with a chatbot, or when content is
   AI-generated/manipulated in a way that isn't obvious
4. **Minimal risk** — no specific AI Act obligations beyond general good
   practice

## Classification of HELLFIRE's own modules

| Module | Tier | Why |
|---|---|---|
| gtm-agent (05) | **Limited risk** | AI drafts outreach content and a qualification score about a prospect. No unaccompanied AI decision reaches a person — every message requires explicit human approval before send (`OutreachDraft.status: draft → approved → sent`, `docs/architecture.md` §4). Sits at limited risk rather than minimal because the *content itself* is AI-generated and sent to an external person; open question below on whether it needs disclosure. Not high-risk: outreach/lead-qualification isn't an Annex III category (it's not employment, credit, law enforcement, etc.) — HELLFIRE isn't deciding anything *about* the prospect that affects their legal or economic access to something. |
| office-agent (06) | **Limited risk** (for external-facing drafts) / **Minimal risk** (for internal knowledge search) | Same logic as gtm-agent for AI-drafted replies to external inquiries once built — human review before send is the load-bearing safeguard. Internal doc search over HELLFIRE/TETA+PI's own knowledge base is minimal risk (no external person affected, no content sent externally as AI-authored). Not yet built — revisit once real scope lands. |
| rag-01 (07) | **Minimal risk** | Infra module — retrieval over internal docs. No external-facing output, no decision about a person. |
| uni-tag (08) | **Minimal risk** | Structured data / llms.txt generation for SEO/GEO visibility — not an interaction with a natural person, not a decision about one. |
| mcp-dev (09) | **Minimal risk** | Developer tooling / integration playbook, not an end-user-facing AI system. |
| inhouse-llm (10) | **Depends on what runs on it, not itself a tier** | The model deployment is infrastructure; classify by what's built on top of it, same as any other module. Currently blocked pre-flight (`STATE.md`, session 10) — no classification needed until scope exists. |
| Verification layer (13) | **Minimal risk** — currently | GitHub-based verification of contractors is evidence-based (public repo history), not an automated profiling/scoring system making decisions about employment eligibility today. **Watch this one** — if it evolves into an automated scoring system that gates contractor pool admission without human review, it moves toward Annex III employment-related high-risk territory. Flagged specifically because this is the module most likely to drift into high-risk without anyone noticing. |
| Nostr time-tracker (14) | **Minimal risk** | Cryptographic signing/logging of work, not an AI system making judgments. |

Modules not yet listed (onboarding, website, internal-db, compliance-layer
itself) are not AI systems making decisions about people in a way the AI Act
addresses — internal-db and onboarding are process/data infrastructure,
website and compliance-layer are not AI decision systems.

## Open question flagged for real legal review (not resolved here)

Whether gtm-agent's AI-drafted, human-approved, human-sent outreach email
triggers Art. 50 transparency obligations is genuinely unclear from this
session's reading: Art. 50 targets chatbot-style interactions and
AI-generated/manipulated content where the AI origin "isn't obvious" — a
one-way marketing email sent by a named human (Bob) under his own identity,
after reviewing and approving the content, sits in ambiguous territory between
"AI-generated content" and "a human-authored email that happened to use an AI
drafting tool." **Do not resolve this by picking whichever answer is more
convenient** — get an actual legal read before Etap 2 turns this
classification table into anything client-facing, and before gtm-agent scales
past manually-curated small lead lists.

## Re-classification triggers

Re-run this classification for a module (don't assume the table above still
holds) if any of the following happens:

- A module starts making or materially influencing a decision *about* a
  specific person that affects their access to employment, credit, essential
  services, or legal rights (this is the direct path into Annex III high-risk).
- A module removes a human-approval gate that this classification relied on
  (e.g. if gtm-agent ever auto-sends without approval — see
  `docs/architecture.md` §4's own framing of the approval gate as a compliance
  safeguard, not just a quality one).
- A module starts interacting directly with external people in a live,
  conversational way (a chatbot) rather than one-shot drafted content — this
  is squarely what Art. 50 was written for and removes the ambiguity flagged
  above.
- HELLFIRE starts selling a module's *template* to a client whose own use case
  falls into a different tier than HELLFIRE's internal dogfooding use (e.g. a
  client uses the office-agent template to draft hiring-rejection
  correspondence — now potentially Annex III territory even though HELLFIRE's
  own internal use of office-agent never was). **This is the most important
  trigger for Etap 2**: risk classification has to be re-run per client
  deployment, not inherited from HELLFIRE's own internal classification.
