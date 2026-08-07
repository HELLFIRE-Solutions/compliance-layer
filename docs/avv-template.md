# Auftragsverarbeitungsvertrag (AVV) — Template

> **STATUS: DRAFT — needs qualified counsel review before use with real
> client data.** This is a non-lawyer's working draft, structured against
> Art. 28 DSGVO's mandatory content list. It is **not** a legally reviewed
> or executable contract. Do not send this to a client, do not attach it to
> an engagement, and do not process any real client personal data under it
> until a qualified data-protection lawyer (ideally German-licensed, given
> HELLFIRE's Berlin entity and `fra1` hosting) has reviewed and signed off.
> See [Task 3 / escalation](#escalation-to-bob-blocking) at the bottom —
> this is the single most important line in the document.

Companion to [standard.md](standard.md), [legitimate-interest.md](legitimate-interest.md),
[data-residency.md](data-residency.md), and [ai-act-classification.md](ai-act-classification.md).
Those documents cover HELLFIRE's *own* internal compliance posture and its
posture as a **data controller** contacting leads. This document is
different: it's the contract HELLFIRE needs *as a data processor*, when a
client engages HELLFIRE to run AI agents (gtm-agent, office-agent, rag-01,
etc.) over **the client's own data about the client's own data subjects**
(e.g. the client's leads, the client's customers, the client's employees —
whoever the client feeds into a HELLFIRE-run agent).

Until now, `data-residency.md` only tracked "verify DPA of our own
*upstream* vendors" (Anthropic, HubSpot) — i.e. HELLFIRE as the customer of
a subprocessor. This document is the missing piece: HELLFIRE as the
**vendor**, needing its own downstream AVV/DPA to offer *to* clients before
any real client data can legally flow into a HELLFIRE-run module.

---

## How to use this template

1. Fill in the bracketed `[...]` fields per client engagement.
2. Attach Annex 1 (subprocessors) and Annex 2 (TOMs) — both are drafted
   below with HELLFIRE's actual current setup, not placeholders, so counsel
   has something concrete to review rather than an empty shell.
3. **Do not execute (sign) any version of this without counsel review.**
   A wrong AVV is worse than no AVV — it creates a paper trail of a
   controller relying on protections that don't actually hold up.
4. Re-derive Annex 1 per client if the subprocessor list differs (e.g. a
   client's engagement doesn't use HubSpot at all) — don't attach a
   subprocessor list that overclaims what's actually used for that client.

---

## Auftragsverarbeitungsvertrag gem. Art. 28 DSGVO

**zwischen**

**[Client legal name and address]**
("Verantwortlicher" / Controller)

**und**

**HELLFIRE Solutions [UG (haftungsbeschränkt) i.G. / GmbH — confirm current
entity form at signing time], [Berlin address]**
("Auftragsverarbeiter" / Processor)

---

### § 1 Gegenstand und Dauer der Verarbeitung (Subject matter and duration)

**(1) Subject matter.** The Processor provides AI-agent-based services to
the Controller under the underlying service agreement dated
`[reference date/number of main contract]`. In the course of providing
these services, the Processor processes personal data on behalf of the
Controller as described in this AVV.

**(2) Scope.** This AVV applies only to the specific HELLFIRE module(s)
in scope for this engagement — check the ones that apply and remove the
rest, since each module touches different data:

- [ ] **gtm-agent** — outbound lead qualification and outreach drafting
- [ ] **office-agent** — inbound reply drafting / internal knowledge search
- [ ] **rag-01** — retrieval-augmented search over the Controller's own
      documents
- [ ] **compliance-layer** (if HELLFIRE runs the audit *for* the client
      rather than as an internal tool) — processes the client's own
      compliance-relevant records
- [ ] Other: `[specify]`

**(3) Duration.** This AVV runs for the duration of the underlying service
agreement and terminates automatically upon its termination, subject to
§ 8 (deletion/return of data) surviving termination.

---

### § 2 Art und Zweck der Verarbeitung (Nature and purpose)

**Nature of processing:** collection, structuring, storage, retrieval,
AI-assisted analysis/drafting, and (where applicable) transmission of
personal data via HELLFIRE's agent modules, run on infrastructure described
in Annex 1.

**Purpose:** `[e.g. "AI-assisted qualification and drafting of outbound
sales outreach on the Controller's behalf" / "AI-assisted drafting of
replies to inbound customer inquiries" / "retrieval-augmented search over
the Controller's internal knowledge base"]` — fill in per engagement, do
not leave generic. The purpose stated here is the outer bound of what the
Processor may do with the data; anything beyond it needs a new instruction
or a new AVV.

---

### § 3 Art der personenbezogenen Daten (Categories of personal data)

Depending on which module(s) are in scope (§ 1(2)), the following
categories may be processed. Strike categories that don't apply to this
specific engagement:

- Contact data: name, email, phone, company, job title
- Communication content: message/email bodies, call notes, meeting notes
- CRM metadata: lead source, lead score, pipeline stage, interaction
  timestamps
- Employment-context data (only if office-agent or a future module touches
  HR-adjacent correspondence — flag to counsel specifically, see
  `ai-act-classification.md`'s Annex III warning on this category)
- Any special category data (Art. 9 DSGVO — health, religion, union
  membership, etc.): **not intended to be processed by any current
  HELLFIRE module.** If a client engagement would involve special category
  data, stop and escalate before proceeding — none of HELLFIRE's current
  modules or this AVV are scoped for Art. 9 data.

---

### § 4 Kategorien betroffener Personen (Categories of data subjects)

- The Controller's leads / prospective customers
- The Controller's existing customers/contacts
- The Controller's employees (only if in scope per § 1(2) — e.g.
  office-agent processing internal correspondence)
- Third parties incidentally referenced in processed content (e.g. a lead
  mentioning a colleague by name in an email) — minimize, do not
  deliberately capture

---

### § 5 Pflichten des Auftragsverarbeiters (Processor obligations)

The Processor (HELLFIRE):

1. Processes personal data **only on documented instructions from the
   Controller** (Art. 29, 32(4) DSGVO), including regarding transfers to a
   third country, unless required by EU or member-state law — in which
   case the Processor informs the Controller of that legal requirement
   before processing, unless the law prohibits such notice.
2. Ensures persons authorized to process the data (HELLFIRE staff,
   verified contractors per HELLFIRE's Nostr-based verification layer) have
   committed to confidentiality.
3. Implements the technical and organizational measures in **Annex 2**.
4. Respects the conditions in § 6 for engaging sub-processors.
5. Assists the Controller, taking into account the nature of processing
   and the information available to the Processor, in responding to data
   subject rights requests (Art. 12–22) and in meeting obligations under
   Art. 32–36 (security, breach notification, DPIA, prior consultation).
6. Notifies the Controller **without undue delay** after becoming aware of
   a personal data breach (target: within 48 hours of confirmation,
   pending counsel review of whether a tighter SLA is warranted).
7. At the Controller's choice, deletes or returns all personal data after
   the end of the service, and deletes existing copies unless EU or
   member-state law requires storage (§ 8).
8. Makes available to the Controller all information necessary to
   demonstrate compliance with Art. 28, and allows for and contributes to
   audits, including inspections, conducted by the Controller or an
   auditor mandated by the Controller (§ 9).

---

### § 6 Unterauftragsverarbeiter (Sub-processors)

**(1) General authorization.** The Controller grants the Processor general
written authorization to engage the sub-processors listed in **Annex 1**.

**(2) New sub-processors.** The Processor informs the Controller of any
intended addition or replacement of sub-processors, giving the Controller
the opportunity to object on reasonable grounds within `[14/30 — set per
engagement]` days.

**(3) Flow-down.** The Processor imposes data protection obligations on
each sub-processor that are no less protective than those in this AVV,
in particular sufficient guarantees for appropriate technical and
organizational measures.

**(4) Liability.** The Processor remains fully liable to the Controller
for the performance of a sub-processor's obligations, where a
sub-processor fails to fulfil its data protection obligations.

---

### § 7 Technische und organisatorische Maßnahmen (TOMs)

See **Annex 2**. TOMs must be reviewed and updated as needed to remain
adequate; material changes are communicated to the Controller.

---

### § 8 Löschung und Rückgabe (Deletion / return of data)

Upon termination of the service (or earlier, on the Controller's request),
the Processor, at the Controller's choice, deletes or returns all personal
data and deletes existing copies, unless EU or member-state law requires
continued storage of the personal data. Deletion/return timeline:
`[e.g. within 30 days of termination]` — set per engagement, confirm
against `internal-db`'s actual backup retention policy
(`internal-db/docs/SCHEMA.md`) before committing to a number, since backup
retention affects how quickly "deletion" is actually achievable.

---

### § 9 Kontrollrechte (Audit rights)

The Controller has the right to verify compliance with this AVV, including
through inspections, either itself or via a mandated third-party auditor,
on reasonable prior notice and during business hours, without
disproportionately disrupting the Processor's operations. The Processor
provides information and documentation reasonably required for this
purpose.

---

### § 10 Haftung (Liability)

`[Standard liability allocation per Art. 82 DSGVO — needs counsel input on
whether to cap liability, and how that interacts with the underlying
service agreement's liability clause. Do not draft a cap without counsel;
an unreviewed cap here could be unenforceable or could conflict with the
main contract.]`

---

### § 11 Internationale Übermittlungen (International transfers)

The Processor's own infrastructure and confirmed sub-processors are listed
in Annex 1 with their data residency status. **Any sub-processor or
infrastructure change that would move personal data outside the EU/EEA
requires**: (a) advance notice to the Controller per § 6(2), and (b) an
appropriate transfer mechanism (adequacy decision, SCCs, or equivalent)
confirmed *before* the transfer, not after. This directly extends the
open gaps already tracked in `data-residency.md` — this AVV is what makes
those gaps contractually load-bearing rather than just an internal note.

---

## Annex 1 — Sub-processors and infrastructure (as of 2026-08-07)

Status pulled directly from [data-residency.md](data-residency.md) — kept
in sync with that document, not redrafted independently. **Re-verify
against the current state of `data-residency.md` before attaching this
annex to any real client AVV** — this snapshot will go stale.

| Sub-processor / infrastructure | Role | Location | DPA/residency status |
|---|---|---|---|
| Shared server (DigitalOcean, region `fra1`) | Hosts `internal-db` (Postgres), module compute | Frankfurt, DE (EU) | Confirmed EU (`data-residency.md`) |
| Anthropic (Claude API) | LLM processing for agent modules (drafting, qualification) | **Not yet verified** — where prompts/outputs are processed, whether current DPA satisfies DSGVO transfer requirements | **Open gap — must be closed before this AVV is executed with a real client.** See `data-residency.md` item 2. |
| HubSpot (gtm-agent's CRM) | Stores lead/contact data for gtm-agent-based engagements | **Not yet created** — EU data hosting is a portal-level setting that must be confirmed at account creation | **Open gap — blocking.** See `data-residency.md` item 1. |
| Email sending provider (SMTP/IMAP, domain TBD) | Outbound/inbound mail transit | TBD | **Open gap.** See `data-residency.md` item 3. |

**This annex cannot be finalized until the two blocking gaps above
(Anthropic DPA verification, HubSpot EU hosting confirmation) are closed.**
An AVV that lists unverified sub-processors as if they were confirmed is
worse than no AVV — flag this explicitly to counsel and to Bob (see
escalation below).

---

## Annex 2 — Technische und organisatorische Maßnahmen (TOMs)

Drafted against HELLFIRE's actual current setup — replace `[...]` items as
they firm up, don't leave this as a generic boilerplate list:

1. **Access control:** role-scoped database credentials (`crm_app`,
   `marketing_app` roles in `internal-db`, per its `scripts/set-app-role-passwords.sh`)
   rather than shared superuser access.
2. **Encryption in transit:** `[confirm TLS everywhere data crosses a
   network boundary — DB connections, HubSpot API, Anthropic API]`.
3. **Encryption at rest:** `[confirm DigitalOcean volume/Postgres
   encryption-at-rest status — not yet confirmed in data-residency.md,
   add to that document's open-gaps list if unconfirmed]`.
4. **Confidentiality commitments:** HELLFIRE staff and verified
   contractors (per `verification-layer`) are bound by confidentiality
   obligations before any client data access.
5. **Backup and retention:** nightly `pg_dump` + retention policy per
   `internal-db/docs/SCHEMA.md` — confirm this document's retention window
   is consistent with § 8's deletion timeline before finalizing.
6. **Human-in-the-loop safeguard:** every AI-drafted external communication
   requires explicit human approval before send (gtm-agent's
   `OutreachDraft.status: draft → approved → sent` gate) — this is both an
   AI Act safeguard (`ai-act-classification.md`) and a data-minimization/
   error-prevention control worth citing here.
7. **Legitimate-interest gate:** outbound contact is blocked in code
   without a recorded legitimate-interest basis (`legitimate-interest.md`,
   `MissingLegitimateInterestError`) — a technical, not just procedural,
   control.
8. **Incident response:** `[document breach-detection and internal
   escalation process — not yet written anywhere in compliance-layer;
   flag as a gap]`.

---

## Escalation to Bob (blocking)

**This AVV template cannot be used with any real client's personal data
as-is.** Concretely, before signing anything based on this template:

1. **A qualified data-protection lawyer must review and finalize this
   document.** Priority items for counsel, in order: liability allocation
   (§ 10, currently a placeholder), breach-notification SLA (§ 5.6,
   currently a guess at 48 hours), deletion timeline (§ 8, currently a
   guess pending confirmation against `internal-db`'s real backup
   retention), and whether HELLFIRE's current entity form (UG vs. GmbH —
   per `project_constitution` memory, conversion is in progress) affects
   how the Processor party should be named.
2. **Two data-residency gaps block Annex 1 from being finalized**, and
   both were already flagged (not new) in `data-residency.md`: HubSpot EU
   hosting must be confirmed *at account creation*, and Anthropic's
   current DPA must be checked against DSGVO transfer requirements. This
   AVV makes those gaps contractually urgent, not just an internal TODO —
   an executed AVV that references unverified sub-processors is a
   liability, not a protection.
3. **This is a blocker for all 3 prospective clients**, not a
   nice-to-have: no real client lead/contact data should flow into
   gtm-agent, office-agent, or rag-01 under a client engagement until a
   counsel-reviewed AVV is signed. This is the same "entry ticket to the
   German market" framing already established for DSGVO generally
   (`standard.md`) — the AVV is the specific document that makes it
   enforceable per client, not just an internal posture.

I can't resolve any of these three myself — they need a lawyer (item 1) and
Bob's direct action on infrastructure decisions (items 2–3). Flagging here
and in the Session Manager report is the extent of what I can do on this.
