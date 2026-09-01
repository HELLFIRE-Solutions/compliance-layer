# Auftragsverarbeitungsvertrag (AVV) — Client-Facing Template

> **STATUS: DRAFT — not yet reviewed and finalized by qualified counsel.**
> This document is provided for discussion purposes only. It is **not** a
> legally reviewed or executable contract, and no personal data should be
> processed under it until both parties' legal counsel have reviewed and
> a final, signed version exists. Open items requiring counsel input are
> listed in the "Open items for counsel" section at the end.

This is the Auftragsverarbeitungsvertrag (data processing agreement)
governing HELLFIRE Solutions' processing of the Controller's personal data
as a processor, under Art. 28 DSGVO, in connection with the AI-agent
services described in the underlying service agreement between the
parties.

---

## How to use this template

1. Fill in the bracketed `[...]` fields for the specific engagement.
2. Review Annex 1 (sub-processors) and Annex 2 (technical and
   organizational measures) — both reflect HELLFIRE's current
   infrastructure and practices as of the date below.
3. This document should not be executed (signed) until both parties'
   counsel have reviewed it.
4. Annex 1 should be re-confirmed for each engagement, since not every
   engagement uses every listed sub-processor.

---

## Auftragsverarbeitungsvertrag gem. Art. 28 DSGVO

**zwischen**

**[Client legal name and address]**
("Verantwortlicher" / Controller)

**und**

**HELLFIRE Solutions [legal form to be confirmed at signing], [Berlin
address]**
("Auftragsverarbeiter" / Processor)

---

### § 1 Gegenstand und Dauer der Verarbeitung (Subject matter and duration)

**(1) Subject matter.** The Processor provides AI-agent-based services to
the Controller under the underlying service agreement dated
`[reference date/number of main contract]`. In the course of providing
these services, the Processor processes personal data on behalf of the
Controller as described in this AVV.

**(2) Scope.** This AVV applies only to the specific service(s) in scope
for this engagement — check the ones that apply:

- [ ] **Outbound lead qualification and outreach drafting**
- [ ] **Inbound reply drafting / internal knowledge search**
- [ ] **Retrieval-augmented search over the Controller's own documents**
- [ ] **Compliance-related documentation support** (where HELLFIRE
      processes the Controller's own compliance-relevant records)
- [ ] Other: `[specify]`

**(3) Duration.** This AVV runs for the duration of the underlying service
agreement and terminates automatically upon its termination, subject to
§ 8 (deletion/return of data) surviving termination.

---

### § 2 Art und Zweck der Verarbeitung (Nature and purpose)

**Nature of processing:** collection, structuring, storage, retrieval,
AI-assisted analysis/drafting, and (where applicable) transmission of
personal data via the Processor's service, run on the infrastructure
described in Annex 1.

**Purpose:** `[e.g. "AI-assisted qualification and drafting of outbound
sales outreach on the Controller's behalf" / "AI-assisted drafting of
replies to inbound customer inquiries" / "retrieval-augmented search over
the Controller's internal knowledge base"]` — to be filled in per
engagement. The purpose stated here is the outer bound of what the
Processor may do with the data; anything beyond it requires a new
instruction or a new agreement.

---

### § 3 Art der personenbezogenen Daten (Categories of personal data)

Depending on which service(s) are in scope (§ 1(2)), the following
categories may be processed. Categories that don't apply to this specific
engagement should be struck:

- Contact data: name, email, phone, company, job title
- Communication content: message/email bodies, call notes, meeting notes
- CRM metadata: lead source, lead score, pipeline stage, interaction
  timestamps
- Employment-context data (only where the service touches HR-adjacent
  correspondence)
- Any special category data (Art. 9 DSGVO — health, religion, union
  membership, etc.): **not intended to be processed under this
  agreement.** If the engagement would involve special category data,
  this must be flagged and addressed separately before any such data is
  processed.

---

### § 4 Kategorien betroffener Personen (Categories of data subjects)

- The Controller's leads / prospective customers
- The Controller's existing customers/contacts
- The Controller's employees (only where in scope per § 1(2))
- Third parties incidentally referenced in processed content (e.g. a lead
  mentioning a colleague by name in an email) — minimized, not
  deliberately captured
- **Minors** — applicable only where the Controller's own data subjects
  include children (e.g. where the underlying service concerns minors, or
  a parent/guardian is the point of contact but the service concerns a
  minor). This category triggers **Art. 8 DSGVO** and, in most such
  engagements, a **mandatory Data Protection Impact Assessment under
  Art. 35 DSGVO**. This box should not be checked, and no processing of
  minors' data should begin, until the required DPIA is in place.
  - [ ] **DPIA required** — completed: `[yes/no, date, reference]`
- **Employees under monitoring** — applicable where the engagement
  involves the Controller's own employee data collected for monitoring
  purposes (e.g. work-time tracking, and/or automated quality-control
  monitoring). This category triggers **§ 26 BDSG** and may require
  works-council (Betriebsrat) involvement on the Controller's side before
  deployment — an obligation of the Controller as employer, which the
  Controller confirms has been or will be addressed before processing
  begins.

---

### § 5 Pflichten des Auftragsverarbeiters (Processor obligations)

The Processor:

1. Processes personal data **only on documented instructions from the
   Controller** (Art. 29, 32(4) DSGVO), including regarding transfers to a
   third country, unless required by EU or member-state law — in which
   case the Processor informs the Controller of that legal requirement
   before processing, unless the law prohibits such notice.
2. Ensures persons authorized to process the data have committed to
   confidentiality.
3. Implements the technical and organizational measures in **Annex 2**.
4. Respects the conditions in § 6 for engaging sub-processors.
5. Assists the Controller, taking into account the nature of processing
   and the information available to the Processor, in responding to data
   subject rights requests (Art. 12–22) and in meeting obligations under
   Art. 32–36 (security, breach notification, DPIA, prior consultation).
6. Notifies the Controller **without undue delay** after becoming aware of
   a personal data breach (target: within 48 hours of confirmation).
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
each sub-processor that are no less protective than those in this AVV, in
particular sufficient guarantees for appropriate technical and
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
`[e.g. within 30 days of termination]` — set per engagement.

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

`[Liability allocation per Art. 82 DSGVO, to be finalized by both parties'
counsel — including how it interacts with the underlying service
agreement's liability clause.]`

---

### § 11 Internationale Übermittlungen (International transfers)

The Processor's own infrastructure and confirmed sub-processors are listed
in Annex 1 with their data residency status. **Any sub-processor or
infrastructure change that would move personal data outside the EU/EEA
requires**: (a) advance notice to the Controller per § 6(2), and (b) an
appropriate transfer mechanism (adequacy decision, SCCs, or equivalent)
confirmed *before* the transfer, not after.

---

## Annex 1 — Sub-processors and infrastructure (as of 2026-08-29)

This annex reflects the Processor's infrastructure and vendor
relationships as of the date above and should be re-confirmed for each
engagement.

| Sub-processor / infrastructure | Role | Location | DPA/residency status |
|---|---|---|---|
| Primary server (cloud hosting provider, EU region) | Hosts the Processor's databases and application compute | Frankfurt, Germany (EU) | Confirmed EU-hosted |
| Anthropic (Claude API) | LLM processing for AI-assisted drafting/qualification features | Data-processing terms and DSGVO transfer basis under confirmation | **Open item — to be closed before this annex is finalized for a real engagement.** |
| CRM platform (for lead/contact-management engagements) | Stores lead/contact data | EU data hosting to be confirmed at account setup | **Open item — to be closed before this annex is finalized for a real engagement.** |
| Email sending provider (for engagements involving outbound/inbound mail) | Mail transit | Provider not yet finalized | **Open item.** |

**This annex cannot be finalized until the open items above are closed.**
A signed AVV that lists unverified sub-processors as if confirmed would
not provide the protection it appears to.

---

## Annex 2 — Technische und organisatorische Maßnahmen (TOMs)

1. **Access control:** role-scoped database credentials rather than shared
   administrative access; access limited to personnel with a defined need.
2. **Encryption in transit:** TLS across network boundaries where personal
   data is transmitted (database connections, third-party API calls).
3. **Encryption at rest:** `[to be confirmed and stated at signing]`.
4. **Confidentiality commitments:** personnel and contractors with access
   to Controller data are bound by confidentiality obligations before any
   access is granted, and are subject to the Processor's own verification
   process for contractor engagement.
5. **Backup and retention:** regular automated backups with a defined
   retention policy, consistent with the deletion timeline in § 8.
6. **Human-in-the-loop safeguard:** AI-drafted external communications
   require explicit human approval before being sent to any third party.
7. **Legitimate-interest gate:** outbound contact initiated without prior
   consent requires a recorded legal basis before any outreach is
   generated — enforced as a technical control, not solely a process
   guideline.
8. **Incident response:** a documented internal escalation process is in
   place (see § 5(6) above for the Controller notification commitment).
   Detection today combines automated network-level monitoring with
   manual review; automated alerting to on-call personnel for all
   detection sources is in progress. `[confirm current state at signing]`.

---

## Open items for counsel

The following items require review and resolution by qualified counsel
for both parties before this document is executed:

1. **Overall legal review and finalization** of this AVV, including
   liability allocation (§ 10), the breach-notification SLA (§ 5(6),
   currently a proposed 48-hour target), the deletion/return timeline
   (§ 8), and confirmation of the Processor's correct legal name and
   entity form at the time of signing.
2. **Two sub-processor items in Annex 1 remain open** and must be closed
   before Annex 1 can be finalized: confirmation of EU data hosting for
   the CRM platform at account setup, and confirmation of the LLM
   provider's data processing terms against DSGVO transfer requirements.
   An executed AVV that references unverified sub-processors would not
   provide the protection it is meant to.
3. **No processing of a real client's personal data should begin under
   this template until a counsel-reviewed, signed version exists** —
   this applies to every category of data subject in § 4, and applies
   with particular weight to the minors and employee-monitoring
   categories given their additional statutory requirements (Art. 8/35
   DSGVO; § 26 BDSG).
