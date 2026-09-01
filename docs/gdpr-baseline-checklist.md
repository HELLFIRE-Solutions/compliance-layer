# GDPR Baseline Checklist

Status: internal working checklist, 2026-08-07, updated 2026-08-29. **Not
a new document from scratch** — this consolidates what
[legitimate-interest.md](legitimate-interest.md),
[data-residency.md](data-residency.md), [ai-act-classification.md](ai-act-classification.md),
[avv-template.md](avv-template.md), and [rdg-legal-services-risk.md](rdg-legal-services-risk.md)
already established, into one practical review pass. Read those for the
*why*; read this one when you need the *status, right now, in one place*.
Not legal advice — see [standard.md](standard.md).

**How to use this:** before onboarding a real client, or periodically as a
health check, go through the checklist below top to bottom. Each item links
back to its source document instead of restating the reasoning. Update the
status column when something changes — this file drifts if it isn't kept
current, same warning `ai-act-classification.md` already gives about its
own table.

---

## 1. Legal basis for contact (Art. 6 DSGVO)

Source: [legitimate-interest.md](legitimate-interest.md)

| # | Check | Status |
|---|---|---|
| 1.1 | Every module that initiates contact without prior consent enforces `LegitimateInterestRecord` in code before outreach, not as a human-followed guideline | ✅ Done for gtm-agent (`MissingLegitimateInterestError` gate) |
| 1.2 | office-agent re-checked once it reaches outbound-contact functionality (not just inbound reply) | ⬜ Not yet built — revisit at build time |
| 1.3 | Audit/export view over `LegitimateInterestRecord` (coverage check, staleness check, opt-out completeness, balancing-test quality) | ⬜ Stage 2 work, spec written, not implemented |
| 1.4 | No automated scraping/messaging on platforms whose ToS prohibits it (LinkedIn) | ✅ Non-goal stated, applies HELLFIRE-wide |

## 2. Data residency (Art. 44–49 DSGVO — international transfers)

Source: [data-residency.md](data-residency.md)

| # | Check | Status |
|---|---|---|
| 2.1 | Primary server region confirmed EU (`fra1`/Frankfurt) | ✅ Confirmed via DO metadata API |
| 2.2 | `internal-db` scoped to EU residency, private repo | ✅ Confirmed |
| 2.3 | HubSpot EU data hosting confirmed **at account creation** | 🔴 **Blocking** — account not yet created; this is the single highest-priority open item in `data-residency.md` |
| 2.4 | Anthropic API DPA verified against DSGVO transfer requirements for prompt/output content | 🔴 **Blocking** — not verified this session or any prior one |
| 2.5 | Email sending domain/provider doesn't route mail through non-EU infra in a way that matters for residency | ⬜ Not yet chosen (domain still TBD per session 02/03) |
| 2.6 | Every future SaaS integration confirms EU hosting/DPA at integration-choice time, recorded in that module's own `docs/architecture.md` | ✅ Process established; applies going forward |

## 3. AI Act risk classification (Regulation (EU) 2024/1689)

Source: [ai-act-classification.md](ai-act-classification.md)

| # | Check | Status |
|---|---|---|
| 3.1 | Every shipping module has a current risk-tier classification | ✅ gtm-agent, office-agent, rag-01, uni-tag, mcp-dev, verification-layer, nostr-tracker classified |
| 3.2 | Human-approval gate in place wherever a module's "limited risk" classification depends on it (gtm-agent's `draft → approved → sent`) | ✅ In code |
| 3.3 | Open legal question — does AI-drafted/human-approved/human-sent outreach trigger Art. 50 transparency obligations | 🟡 **Genuinely unclear, flagged for real legal review** — not resolved, do not guess |
| 3.4 | Verification-layer (module 13) re-checked if it evolves toward automated scoring that gates contractor admission without human review (Annex III drift risk) | ⬜ Watch item, no current trigger |
| 3.5 | **Risk classification re-run per client deployment**, not inherited from HELLFIRE's internal use (a client's own use case, e.g. hiring-adjacent correspondence, can land in a different tier than HELLFIRE's internal dogfooding) | 🔴 **Not yet operationalized** — no process exists today to re-classify per client before an engagement starts. Needed before any of the 3 prospective clients goes live. |

## 4. Processor relationship with clients (Art. 28 DSGVO)

Source: [avv-template.md](avv-template.md) — new as of this pass

| # | Check | Status |
|---|---|---|
| 4.1 | AVV/DPA template exists for HELLFIRE-as-processor engagements | 🟡 **Draft exists**, needs qualified counsel review before any real use — see `avv-template.md` |
| 4.2 | AVV executed with each real client before that client's personal data enters any HELLFIRE module | 🔴 **Blocking for all 3 prospective clients** — no AVV has been reviewed by counsel or signed with anyone yet |
| 4.3 | Sub-processor annex (Annex 1 of the AVV) kept in sync with `data-residency.md`, re-verified per client before attaching | 🔴 Cannot be finalized until 2.3 and 2.4 above are closed |
| 4.4 | TOMs (Annex 2 of the AVV) — encryption in transit/at rest, incident response process | 🟡 Incident response now documented ([incident-response.md](incident-response.md)); encryption-at-rest status for the primary server still unconfirmed |
| 4.5 | § 4 (categories of data subjects) covers all prospective clients' actual use cases, not just the generic default categories | ✅ Updated 2026-08-29 to add minors (Art. 8/DPIA) and monitored-employees (§ 26 BDSG) — see `avv-template.md`'s changelog |
| 4.6 | Sanitized, client-facing version of the AVV exists, separate from the internal working draft | ✅ [avv-template-external.md](avv-template-external.md), added 2026-08-29 |

## 5. Deletion, retention, and data subject rights

Not fully owned by any single existing document — surfaced here as a gap,
not previously tracked as a checklist item anywhere in compliance-layer.

| # | Check | Status |
|---|---|---|
| 5.1 | Backup retention policy (`internal-db/docs/SCHEMA.md`) is consistent with whatever deletion timeline ends up in a signed AVV | ⬜ Not cross-checked yet — do this before setting a number in any AVV § 8 |
| 5.2 | Process for responding to a data subject access/erasure request | ⬜ Not documented anywhere in compliance-layer today — real gap |
| 5.3 | Breach notification process/SLA (internal escalation, not just the AVV's contractual promise to the client) | ✅ Documented 2026-08-29 — [incident-response.md](incident-response.md). Real gap remains inside that process: no external alerting channel yet, detection is partly manual. |

## 6. Records of processing activities (Art. 30(2) DSGVO)

Source: new gap, identified by outside counsel 2026-08-29 — not
previously tracked anywhere in this project.

| # | Check | Status |
|---|---|---|
| 6.1 | A processor's record of processing activities exists (Art. 30(2): categories of processing carried out on behalf of each controller, per-controller contact details, categories of data subjects/data, transfers, TOM reference, retention where known) | 🔴 **Does not exist.** HELLFIRE has never maintained this record for any module. Required once HELLFIRE processes personal data on behalf of any controller as a processor — i.e. as soon as the first real client AVV is signed, not before, but it must exist by then, not be built reactively after an audit request. |
| 6.2 | Record kept current as new client engagements start/change/end | ⬜ N/A until 6.1 exists |

Note: this is distinct from Art. 30(1) (a **controller's** own record,
which would cover HELLFIRE's own lead data in gtm-agent) — that has also
never been formally compiled, but wasn't the specific gap counsel flagged.
Worth a follow-up check, not conflated with 6.1 here.

## 7. RDG / legal-services classification (Stage 2 only)

Source: [rdg-legal-services-risk.md](rdg-legal-services-risk.md) — new
2026-08-29, does not affect Stage 1.

| # | Check | Status |
|---|---|---|
| 7.1 | Stage 2 (sellable compliance-audit product) confirmed not to require RDG licensing | 🔴 **Blocking Stage 2.** Separate counsel opinion requested 2026-08-29, not yet received. Do not start Stage 2 build work regardless of schedule pressure. |
| 7.2 | Mitigations designed independent of the opinion's outcome (draft-only output labeling, lawyer-approval gate, marketing-copy constraint, output provenance logging) | ✅ Specified in `rdg-legal-services-risk.md` "Mitigations" section, 2026-08-29 — design only, Stage 2 has no code yet |
| 7.3 | Marketing copy across the project checked for lawyer-comparison / "law-firm-quality" claims (independently risky per the Smartlaw marketing injunction, regardless of the RDG outcome) | ✅ Checked 2026-08-29 (website pages, this module's README) — no violations found. Re-check whenever new compliance-adjacent marketing copy is written. |

---

## Summary — what's actually blocking real client work

Reading the 🔴 rows together (2.3, 2.4, 3.5, 4.2/4.3, 6.1, 7.1): **no real
client personal data should enter gtm-agent, office-agent, or rag-01, and
no Stage 2 compliance-audit product should be built or sold, until:**

1. HubSpot account created with EU hosting confirmed, **or** that specific
   client engagement doesn't use HubSpot at all.
2. Anthropic's DPA verified against DSGVO transfer requirements (one-time
   check, not per-client).
3. A per-client AI Act re-classification step exists and is run before
   that client's engagement starts.
4. An AVV — reviewed by qualified counsel, not the current draft as-is —
   is signed with that specific client.
5. An Art. 30(2) processor record exists and is kept current per client.
6. **Separately, and specific to Stage 2 only:** the pending RDG opinion
   comes back, confirming Stage 2's design (as mitigated) does not require
   legal-services licensing — or a redesign counsel confirms resolves it.

This is the same conclusion `data-residency.md`, `avv-template.md`, and
`rdg-legal-services-risk.md` independently point to; this checklist just
puts it in one place instead of requiring someone to cross-reference
several documents to see it.
