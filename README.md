# HELLFIRE AI Solutions — Compliance / Trust Layer

Module 7. Documents our own compliance approach (legitimate interest, data residency, AI Act risk classification) as an internal standard, based on TETA+PI's TWIRA verification logic. DSGVO/AI Act — the entry ticket to the German market.

**Dogfooding → template:** a compliance audit + rollout checklist/framework, sellable as a standalone module or a required add-on to other modules. **This Stage 2 plan is currently blocked — see [`docs/rdg-legal-services-risk.md`](docs/rdg-legal-services-risk.md).**

**Documentation (Stage 1, 2026-07-21):**
- [`docs/standard.md`](docs/standard.md) — HELLFIRE Internal Compliance Standard v1, overview and coordination with gtm-agent/TWIRA
- [`docs/legitimate-interest.md`](docs/legitimate-interest.md) — DSGVO Art. 6(1)(f), based on `gtm-agent`'s `LegitimateInterestRecord`
- [`docs/data-residency.md`](docs/data-residency.md) — EU data residency, including open gaps (HubSpot, Anthropic API)
- [`docs/ai-act-classification.md`](docs/ai-act-classification.md) — risk tier for every HELLFIRE module

**Added 2026-08-07 (real-client-data blocker close-out pass):**
- [`docs/avv-template.md`](docs/avv-template.md) — **DRAFT, needs qualified counsel review** — Art. 28 DSGVO AVV/DPA template for engaging HELLFIRE as a processor of client data. Previously there was no such template at all (only "verify our own vendors' DPAs" in `data-residency.md`); this is the missing client-facing side.
- [`docs/gdpr-baseline-checklist.md`](docs/gdpr-baseline-checklist.md) — consolidated practical checklist over the four documents above, with a live status per item and a summary of what's actually blocking real client engagements right now.

**Added 2026-08-29 (counsel review of the AVV surfaced a second, separate blocker):**
- [`docs/rdg-legal-services-risk.md`](docs/rdg-legal-services-risk.md) — **BLOCKING GATE on Stage 2 specifically.** Counsel flagged that an LLM-based *individualized* compliance assessment (the Stage 2 plan above) risks being an unlicensed `Rechtsdienstleistung` under German RDG § 2 — distinguished from the `BGH I ZR 113/20` ("Smartlaw") precedent, which cleared a tool using predefined decision trees and standardized clauses rather than case-by-case legal judgment. Stage 1 (internal use, this README's own documentation) is unaffected. A separate counsel opinion has been requested and is not yet back — do not start Stage 2 build work until it clears.
- [`docs/avv-template.md`](docs/avv-template.md) also gained a sanitized client-facing sibling, [`docs/avv-template-external.md`](docs/avv-template-external.md), and § 4 now covers minors and monitored-employee data subjects (see that file's own changelog note).
- [`docs/incident-response.md`](docs/incident-response.md) — the breach-detection/escalation process that was previously just a TODO in the checklist and the AVV's Annex 2. Documents what detection actually exists today (firewall/fail2ban, health checks) and what doesn't (no external alerting channel yet — flagged as an open gap, not solved by writing the process down).
- [`docs/gdpr-baseline-checklist.md`](docs/gdpr-baseline-checklist.md) gained two new sections: Art. 30(2) processor records (a gap not previously tracked anywhere in this project) and RDG/legal-services classification status.

**Status:** Stage 1 done (documentation of our own compliance approach), plus the AVV-template gap closed as a draft (counsel review still required). **Stage 2 (sellable audit framework) not started, and now formally blocked pending the RDG opinion — not just "not started" for lack of time.**

**License:** MIT.
