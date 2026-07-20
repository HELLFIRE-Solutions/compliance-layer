# Legitimate Interest Standard (DSGVO Art. 6(1)(f))

Status: adopted from gtm-agent (session 05), 2026-07-21. This is not a new
schema — see [standard.md](standard.md) "Coordination record."

## The rule

Any HELLFIRE module that initiates contact with a natural person **without
their prior consent** must record a legitimate-interest basis for that contact
*before* any outreach is generated, and must enforce this in code — not as a
process guideline a human can forget to follow. This is the pattern gtm-agent
already built (`Orchestrator.qualify_and_draft` raises
`MissingLegitimateInterestError` if the record is missing or the contact has
opted out — `gtm-agent/src/gtm_agent/models.py`).

Modules this currently applies to: **gtm-agent** (built). Modules it will
apply to once they reach outbound-contact functionality: **office-agent**, if
its automated-reply drafting ever initiates contact with someone who hasn't
already written in (replying to an inbound email is not itself a Art. 6(1)(f)
question in the same way — the contact-initiation trigger is what matters,
not "does this module use AI").

## Canonical schema

Reuse `gtm_agent.models.LegitimateInterestRecord` unchanged. Do not fork it or
redefine an equivalent structure in another module or in `internal-db`. The
fields, and why each exists:

| Field | Purpose |
|---|---|
| `source_type` | Where the lead came from (`linkedin_connection`, `referral`, `event`, `inbound_engagement`, `other`) |
| `source_detail` | The concrete fact establishing a pre-existing connection — required, non-empty. Not "found on LinkedIn"; "mutual connection Jane Doe" or "commented on HELLFIRE's DSGVO post, 2026-06-01." This is what separates legitimate-interest outreach from mass/unsolicited contact. |
| `legal_basis` | Fixed string, `"Art. 6(1)(f) GDPR – legitimate interest"` — a placeholder field so a future module or client deployment can swap in a different basis (e.g. consent) without a schema change |
| `balancing_test_notes` | The actual Art. 6(1)(f) balancing test in writing: why HELLFIRE's interest doesn't override the contact's rights. Required, non-empty — not a formality field |
| `recorded_by` / `recorded_at` | Accountability — who logged it, when |
| `opt_out_at` | Nullable; once set, `Lead.require_legitimate_interest()` refuses further contact for that person, permanently |
| `review_date` | Periodic re-validation trigger — a legitimate-interest basis from years ago with no recent interaction is weaker grounds and should be re-assessed, not assumed to hold forever |

Synced to HubSpot as contact properties (`legitimate_interest_source`,
`legitimate_interest_basis`, `legitimate_interest_recorded_at`,
`legitimate_interest_notes`) so the record lives on the contact itself, visible
to anyone with CRM access — not buried in a side system.

## What compliance-layer adds in Etap 1 (not a schema change)

gtm-agent's own architecture doc (`gtm-agent/docs/architecture.md` §3) already
flags this as "intentionally minimal and Etap-1-scoped" and expects
compliance-layer to add an audit/export view rather than change capture. Etap 1
here defines what that view needs to answer, for Etap 2 to build against:

- **Coverage check:** does every contact with `status != draft` have an active
  (non-opted-out) `LegitimateInterestRecord`? (Should always be true given the
  code-level gate — this check exists to catch drift if the gate is ever
  bypassed or the schema is extended elsewhere without going through it.)
- **Staleness check:** which records have `review_date` in the past, or no
  `review_date` at all, and haven't been re-validated?
- **Opt-out completeness:** for any `opt_out_at` record, confirm no draft or
  send exists with `created_at`/`sent_at` after the opt-out timestamp.
- **Balancing-test quality:** flag records where `balancing_test_notes` is
  suspiciously short (e.g. under ~20 characters) for human re-review — a
  non-empty string isn't the same as a real balancing test.

This is a read/report layer over gtm-agent's existing data (via HubSpot
properties or the local store), not a new write path. Building it is Etap 2
work (compliance-audit product), tracked here as the spec, not implemented yet.

## Non-goal (inherited from gtm-agent, applies HELLFIRE-wide)

No automated scraping or automated messaging on platforms whose ToS prohibits
it (LinkedIn, specifically). Legitimate-interest documentation for a contact
someone manually identified is a different legal question from automating the
identification/contact step itself — the latter is out of scope for every
HELLFIRE module, not just gtm-agent.
