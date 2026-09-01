# RDG Legal-Services Risk (Working Standard — Blocking Gate)

Status: internal working standard, 2026-08-29, based directly on outside
counsel's review of `avv-template.md` (full detail: `STATE.md` "Session 11
(continued) — counsel review of AVV, RDG finding"). **Not legal advice** —
same caveat as `ai-act-classification.md`: this is a non-lawyer's working
write-up of counsel's flagged concern, structured so the blocking status is
impossible to miss, not a substitute for the separate counsel opinion this
document is itself waiting on.

## The rule

`compliance-layer` Stage 2 — the sellable "compliance audit + rollout
checklist/framework" this module's own [README.md](../README.md) has
described since Stage 1 — **may not ship until a qualified opinion confirms
it does not constitute an unlicensed legal service under German law.**
This is a hard gate, same weight as `data-residency.md`'s HubSpot/Anthropic
items: Stage 2 work does not proceed past design/documentation while this
gate is open, the same way no real client data may flow into an unverified
sub-processor.

**This gate does not affect Stage 1.** Internal documentation of HELLFIRE's
own compliance posture (`standard.md`, `legitimate-interest.md`,
`data-residency.md`, `ai-act-classification.md`, `avv-template.md` itself)
is HELLFIRE assessing its own compliance, not selling a legal-assessment
product to a third party. The distinction below is exactly what makes that
true today and what would stop being true if Stage 2 shipped unchanged.

## § 2 RDG, in brief

Germany's `Rechtsdienstleistungsgesetz` (RDG) restricts who may provide
"Rechtsdienstleistung" — legal services — as a business. § 2(1) defines a
Rechtsdienstleistung as: **the examination of an individual, actual case
(Prüfung eines konkreten Einzelfalls)**, applied against legal norms, that
requires a legal assessment going beyond general information. Providing
this without the required license (Rechtsanwaltszulassung, or a narrower
registration under § 10 for specific fields) is prohibited. The point of
the statute is consumer/client protection: someone receiving what looks
like a legal judgment about their specific situation should be able to
rely on it having come from someone qualified and liable for it.

## `BGH I ZR 113/20` ("Smartlaw", 9 Sep 2021) — what it actually decided

The Bundesgerichtshof held that Smartlaw's online contract-generator was
**not** an unlicensed Rechtsdienstleistung. The reasoning turned on two
specific facts, not on "it's software" or "it's not a human lawyer":

1. **Predefined decision trees, standardized clauses.** The tool asked the
   user a fixed sequence of questions and assembled a document from a
   pre-authored library of clause variants selected by branching logic
   written and reviewed in advance. There was no step where the tool
   evaluated the user's specific facts against legal norms and reached a
   novel conclusion about *that* user's case — every possible output
   already existed, pre-approved, before any user ever ran the tool.
2. **No expectation of individualized legal judgment.** A user of a
   contract generator understands they're assembling a document from
   options, not receiving a lawyer's assessment of their specific
   situation — the court weighed this expectation as part of why the
   output wasn't functioning as a Rechtsdienstleistung in practice.

Separately — and independent of the RDG holding — the court permanently
enjoined Smartlaw's own marketing claims that its tool was "cheaper/faster
than a lawyer" and produced "law-firm-quality" documents, as misleading.
This second finding does not depend on how the RDG question is ultimately
resolved; see "Marketing constraint" below.

## Why compliance-layer Stage 2, as currently described, sits on the wrong side of that line

`compliance-layer`'s own README describes Stage 2 as delivering "a
compliance audit + rollout checklist/framework" — and the module's
underlying mechanism (an LLM, per `ai-act-classification.md`'s treatment of
every other HELLFIRE module) would generate that audit by reading a
specific client's actual data, infrastructure, and processing activities,
and producing conclusions about *that client's* compliance posture. That is
the individualized-assessment pattern Smartlaw's tool specifically avoided:

| | Smartlaw (not RDG) | compliance-layer Stage 2 as described (risk) |
|---|---|---|
| Input | Fixed multiple-choice questions | A specific client's actual case: their data flows, vendors, contracts |
| Output construction | Assembled from a pre-authored, pre-reviewed clause library via branching logic | LLM-generated assessment produced at query time, not pre-authored and reviewed per possible output |
| What the output represents | "Here is the standardized document matching your selections" | "Here is our assessment of whether/how *you* comply" — an individualized legal judgment |
| User's expectation | Assembling a document from a menu | Receiving a compliance evaluation of their specific business |

The gap is not "LLM vs. rules engine" as a technology distinction — it's
whether the system's output is a pre-approved artifact selected by fixed
logic, or a fresh legal conclusion about one client's actual facts. As
currently scoped, Stage 2 is the latter. This is why counsel flagged it as
a *separate* issue from the AVV review, and why a design that keeps
Stage 2 on the Smartlaw side of the line (see mitigations below) is worth
pursuing regardless of what the pending opinion says — it either resolves
the concern outright or narrows what the opinion needs to bless.

## Mitigations — design now, independent of the pending opinion

Counsel's instruction: these are design changes that reduce RDG surface
regardless of what the separate opinion concludes — they either help
resolve the concern outright, or narrow what the opinion needs to bless.
They are documentation/design requirements for Stage 2 **before any Stage 2
code is written**, not a description of anything currently built (Stage 2
has not started).

1. **Output must be labeled draft/checklist, never a legal conclusion.**
   Every output Stage 2 ever produces — once built — must be framed as a
   draft or a checklist item for human review, never as "this is compliant"
   or "this satisfies your legal obligation." This is a direct echo of the
   Smartlaw distinction: a system whose output is explicitly presented as
   unreviewed material for a human to check is functioning differently,
   both legally and in the user's expectation, from one presenting a
   finished judgment. **This requirement must be written into Stage 2's own
   design document before implementation starts** — it is not something to
   retrofit onto UI copy after the fact.

2. **Mandatory human-lawyer approval gate before client delivery.** Stage 2
   needs the same shape of gate gtm-agent already enforces in code for
   outreach — `OutreachDraft.status: draft → approved → sent`
   (`gtm-agent/docs/architecture.md`). The Stage 2 equivalent, to be
   specified in Stage 2's design doc and enforced in code once built:

   ```
   ComplianceOutput.status: draft → lawyer_reviewed → released
   ```

   - `draft`: LLM-generated, exists only internally, never visible to the
     client.
   - `lawyer_reviewed`: a qualified human reviewer has read the specific
     output and approved it for that client — recorded with
     reviewer identity and timestamp, mirroring
     `LegitimateInterestRecord.recorded_by`/`recorded_at`'s accountability
     pattern (`legitimate-interest.md`).
   - `released`: only a `lawyer_reviewed` output may transition here and
     reach the client. No code path may skip `lawyer_reviewed` — same
     principle as gtm-agent's `MissingLegitimateInterestError` gate: a
     structural block, not a process a human could forget to follow.

   This gate is a mitigation regardless of the RDG outcome: if the opinion
   comes back favorable, the gate is still good practice (same rationale
   `ai-act-classification.md` already gives for gtm-agent's human-approval
   gate as a compliance safeguard, not just a quality one). If it comes back
   unfavorable, the gate is very likely a required part of whatever
   redesign counsel asks for.

3. **Marketing-copy constraint: no lawyer-comparison or "law-firm-quality"
   claims, anywhere, regardless of the RDG outcome.** Smartlaw's own
   marketing ("cheaper/faster than a lawyer," "law-firm-quality documents")
   was permanently enjoined as misleading — a finding independent of the
   RDG question. This is a hard constraint on any current or future
   marketing copy for `compliance-layer` (or any module whose output
   resembles legal work product): no comparison to lawyers/law firms, no
   claims of legal-professional-equivalent quality.

   **Checked 2026-08-29, this session:** `website/src/pages/index.astro`,
   `website/src/pages/partners.astro`, `website/src/pages/treasury.astro`,
   `website/src/pages/legal.astro`, `website/src/components/Footer.astro`,
   `website/src/layouts/Layout.astro`, and this module's own
   `compliance-layer/README.md` — searched for lawyer/law-firm/Anwalt/
   Rechtsdienstleistung comparisons and quality claims (`cheaper than`,
   `faster than`, `law-firm-quality`, `instead of a lawyer`, `expert-level`,
   `professional-grade`, etc.). **No violations found.** The one
   compliance-related line on the site (`index.astro`'s ideal-customer-
   profile bullet, "Where GDPR / AI Act compliance isn't a nice-to-have but
   a requirement of any vendor") describes a customer segment, not a claim
   about HELLFIRE's own output quality — not a concern. Re-check this list
   whenever new marketing copy is written for compliance-layer or any
   module producing legal-adjacent output, since this constraint doesn't
   go away if the RDG opinion is favorable.

4. **Output provenance/traceability logging (spec now, implement in
   Stage 2 build).** Every Stage 2 output must log which template version
   and which specific set of clauses/criteria produced it — not
   necessarily code today, but a requirement Stage 2's design doc must
   commit to before implementation, so it isn't bolted on after the first
   client output ships. Concretely, once built: each `ComplianceOutput`
   record should carry `template_version`, `criteria_set_id` (or
   equivalent), `generated_at`, and the reviewer fields from mitigation 2.
   This serves two purposes: it is the evidentiary record a lawyer's
   `lawyer_reviewed` sign-off is actually reviewing against (you can't
   approve "the output" without knowing which version of the underlying
   logic produced it), and it is the kind of audit trail that supports
   arguing Stage 2 behaves more like a version-controlled document-assembly
   system (Smartlaw's side of the line) than an opaque one-off judgment.

## Status: blocking

- **Blocks:** Stage 2 of `compliance-layer` (the sellable audit/checklist
  product) — no build, no sale, no client-facing delivery of an
  individualized compliance assessment until this gate clears.
- **Does not block:** Stage 1 (this module's existing internal
  documentation), or any other HELLFIRE module's Stage 1/2 work.
- **Cleared by:** a qualified counsel opinion confirming Stage 2's design
  (as mitigated, see below) does not constitute a Rechtsdienstleistung
  under RDG § 2 — or a redesign counsel confirms moves it back to the
  Smartlaw side of the line (e.g. constraining Stage 2 to a fixed
  checklist/scorecard against pre-authored criteria, with no free-form
  individualized legal conclusion in the output).
- **Requested, not yet received:** Session Manager sent counsel a separate
  request for this specific opinion 2026-08-29 (same day as the AVV
  review response). Not yet answered as of this writing.
- **Do not resolve by picking whichever answer is convenient** — same
  standing instruction `ai-act-classification.md` already gives for its
  own open legal question. If work needs to proceed before the opinion
  arrives, it must be the mitigation work in
  [standard.md](standard.md)'s Stage 2 section (see below), which reduces
  RDG surface regardless of the outcome — not Stage 2 feature work that
  assumes the answer will be favorable.

## Re-classification triggers

Re-check this gate's status if:

- The counsel opinion arrives (obviously) — record the outcome here, not
  just in `STATE.md`, so this document stays the single source of truth
  for whether Stage 2 may proceed.
- Stage 2's actual design changes in a way that moves it toward or away
  from the Smartlaw pattern (e.g. constraining output to a fixed
  checklist/scorecard against pre-authored criteria narrows the risk;
  adding free-form "here's what you should do" legal recommendations
  widens it).
- HELLFIRE considers selling compliance-layer's Stage 2 output into a
  jurisdiction other than Germany — RDG is German law specifically; a
  different jurisdiction's unauthorized-practice-of-law rules would need
  their own review, not an assumption that a German-cleared design is
  automatically safe elsewhere.
