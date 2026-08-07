# HELLFIRE AI Solutions — Compliance / Trust Layer

Module 7. Documents our own compliance approach (legitimate interest, data residency, AI Act risk classification) as an internal standard, based on TETA+PI's TWIRA verification logic. DSGVO/AI Act — the entry ticket to the German market.

**Dogfooding → template:** a compliance audit + rollout checklist/framework, sellable as a standalone module or a required add-on to other modules.

**Documentation (Stage 1, 2026-07-21):**
- [`docs/standard.md`](docs/standard.md) — HELLFIRE Internal Compliance Standard v1, overview and coordination with gtm-agent/TWIRA
- [`docs/legitimate-interest.md`](docs/legitimate-interest.md) — DSGVO Art. 6(1)(f), based on `gtm-agent`'s `LegitimateInterestRecord`
- [`docs/data-residency.md`](docs/data-residency.md) — EU data residency, including open gaps (HubSpot, Anthropic API)
- [`docs/ai-act-classification.md`](docs/ai-act-classification.md) — risk tier for every HELLFIRE module

**Added 2026-08-07 (real-client-data blocker close-out pass):**
- [`docs/avv-template.md`](docs/avv-template.md) — **DRAFT, needs qualified counsel review** — Art. 28 DSGVO AVV/DPA template for engaging HELLFIRE as a processor of client data. Previously there was no such template at all (only "verify our own vendors' DPAs" in `data-residency.md`); this is the missing client-facing side.
- [`docs/gdpr-baseline-checklist.md`](docs/gdpr-baseline-checklist.md) — consolidated practical checklist over the four documents above, with a live status per item and a summary of what's actually blocking real client engagements right now.

**Status:** Stage 1 done (documentation of our own compliance approach), plus the AVV-template gap closed as a draft (counsel review still required). Stage 2 (sellable audit framework) not started.

**License:** MIT.
