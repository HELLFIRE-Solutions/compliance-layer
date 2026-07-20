# Data Residency Standard

Status: internal standard, 2026-07-21. Not legal advice — see
[standard.md](standard.md) "What Etap 1 deliberately does not do."

## The rule

Personal data of EU data subjects (leads, contacts, clients, contractors)
that HELLFIRE stores or processes must stay within the EU for as long as
DSGVO applies to it. This was already stated as a hard architectural
requirement in the original project plan (`project-overview` memory: "DSGVO/EU
data residency is treated as a hard requirement... since it's the entry ticket
to the German market") — this document is the first place it's written down
as a standard modules are checked against, rather than an assumption.

## What's actually confirmed today

- **Server:** shared DigitalOcean droplet, region `fra1` (Frankfurt),
  confirmed via DO metadata API in session 02 (`STATE.md`, "Session 02 —
  Server/DevOps"). This is where `internal-db`'s Postgres will run and where
  any module's own compute/storage runs unless a module explicitly uses an
  external SaaS.
- **Precedent:** TETA+PI (sibling project, shares this server) is itself
  incorporated as TetaPi GmbH in Frankfurt am Main and hosts on the same
  `fra1` infrastructure (`TETA+PI/docs/overview.md`) — HELLFIRE's residency
  posture is consistent with, not weaker than, the project it dogfoods
  alongside.
- **`internal-db`:** private repo, explicitly scoped to EU residency in its own
  README ("Містить дані клієнтів — репозиторій приватний, EU data residency
  обов'язкова (DSGVO)") and in `STATE.md` row 04. Consistent with this
  standard, already decided independently — no conflict to resolve.

## Open gaps — flag before they become incidents, not after

These are the concrete items compliance-layer's Etap 1 audit surfaced by
checking what's *actually* configured, not just what's planned:

1. **HubSpot (gtm-agent's CRM, `STATE.md` row 05):** not yet created (blocked
   on account access). HubSpot's data-hosting region is a portal-level setting
   chosen at account creation, and EU data hosting has historically been a
   paid-tier feature, not guaranteed on Free. **Action for whoever creates the
   HubSpot account:** confirm the EU data hosting option before entering any
   real lead data, not after. This is the single highest-priority open item
   from this audit — it's cheaper to get right at signup than to migrate later.
2. **Anthropic API (used by gtm-agent's orchestrator, and by every future
   module that calls Claude):** verify Anthropic's current data processing
   terms/DPA for where prompts and outputs are processed, and whether that
   satisfies DSGVO transfer requirements for the personal data HELLFIRE sends
   in prompts (e.g. lead name/company/context passed to Claude for
   qualification and drafting). Not verified in this session — needs a
   one-time check against Anthropic's current DPA, not a per-module recheck.
3. **Email sending domain (`hellfire.dev` / `hellfiresol.com`, tracked under
   session 02/03):** once live, confirm the sending provider (SMTP/IMAP per
   `gtm-agent/docs/architecture.md`) doesn't route mail through non-EU
   infrastructure in a way that matters for residency — mail transit is lower
   risk than data-at-rest, but worth a one-line confirmation when that
   provider is chosen.
4. **Future SaaS integrations (any module):** the same check HubSpot needs
   applies to every future third-party service a module integrates with —
   confirm EU hosting/DPA *before* real personal data flows into it, as part
   of that module's own build session, not deferred to a later compliance
   pass.

## How future modules should apply this

When a module's build session picks a third-party service that will touch
personal data (CRM, email provider, vector DB, analytics, anything), that
session should record in its own `docs/architecture.md` — same pattern
gtm-agent used for CRM choice — a one-line confirmation of that service's EU
data residency posture, alongside the existing cost/functionality rationale.
Don't defer this check to compliance-layer as a separate later audit; catching
it at integration-choice time is cheaper than catching it in an Etap 2 audit
after real data is already in a non-EU system.
