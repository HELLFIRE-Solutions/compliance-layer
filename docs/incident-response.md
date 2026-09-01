# Incident Response / Breach Detection Process

Status: internal working process, 2026-08-29. Written to close the gap
counsel confirmed as needed before any real client engagement
(`gdpr-baseline-checklist.md` item 4.4, `avv-template.md` Annex 2 item 8 —
both previously said "not yet written," this document is that write-up).
Not legal advice — this is an operational process, not a legal opinion on
what Art. 33/34 DSGVO requires in a specific incident; get counsel input
when an actual incident happens, don't rely solely on this document to
decide whether/how to notify.

## Why this exists

A processor AVV (`§ 5.6` of [avv-template.md](avv-template.md)) commits
HELLFIRE to notify a Controller "without undue delay" after becoming aware
of a personal data breach, with a target SLA. That commitment is only
real if there's an actual process behind it — detection, internal
escalation, containment, and a documented notification path. Before this
document, that process didn't exist anywhere in `compliance-layer`; the
AVV referenced it as a TODO.

## What "breach" means here

A personal data breach, per Art. 4(12) DSGVO: a breach of security leading
to accidental or unlawful destruction, loss, alteration, unauthorized
disclosure of, or access to, personal data. Concretely, for HELLFIRE's
current setup, this includes (non-exhaustive):

- Unauthorized access to `internal-db` (the Postgres instance holding
  client/contact/contract data)
- Compromise of the shared `fra1` server or any container running on it
- Leaked or compromised credentials for HubSpot, the Anthropic API, email
  sending accounts, or `internal-db`'s scoped roles (`crm_app`,
  `marketing_app`)
- A HELLFIRE-authored bug that exposes one client's data to another (cross-
  tenant leakage) — relevant once compliance-layer or any module serves
  multiple clients from shared infrastructure
- Loss of a device (laptop, phone) with access to client data or
  credentials
- A contractor (verified via `verification-layer`) mishandling or
  improperly retaining client data outside authorized systems

## Detection sources — what exists today, and what doesn't

**Exists today:**
- `ufw` firewall + `fail2ban` (3 jails) on the shared `fra1` server —
  blocks/flags brute-force and unauthorized-access attempts at the network
  level.
- `/opt/hellfire/healthcheck/check.sh`, run every 5 minutes via cron —
  checks site availability and container health (`tetapi-postgres`,
  `tetapi-redis`, `internal-db-db-1`), logs to `health.log`, failures to
  `alerts.log`.

**Does not exist today — real gaps, not covered by simply writing this
process:**
- **No external alerting channel.** `alerts.log` is a file Bob can tail;
  nothing pages or emails anyone when it's written to. The health-check
  system itself already flagged this ("no channel exists to alert *to*
  until the mailbox... is set up"). This means detection today is
  **manual/reactive** (someone notices something wrong) rather than
  automated for most breach scenarios — the firewall/fail2ban layer is the
  exception, since it acts (blocks) rather than just logs.
- **No database access/query audit logging** on `internal-db` beyond
  standard Postgres logs — no automated anomaly detection for unusual
  access patterns (e.g. a bulk export of contact rows).
- **No centralized log aggregation** — logs are per-service, on the one
  shared server, not shipped anywhere durable/searchable.

**Action, not optional:** once the mailbox (`hellfiresol.com`, provider
still pending per `STATE.md`) exists, wire `alerts.log` failures to it, so
detection stops depending on someone manually tailing a file. This is the
single highest-leverage fix to this document's biggest actual gap and
should happen as part of whichever session sets up the mailbox.

## Internal escalation path

1. **Whoever detects a suspected incident** (a dev session noticing
   something wrong, Bob noticing an alert, a contractor reporting
   something) **stops what they're doing and escalates immediately** — do
   not attempt to quietly fix and move on. Report to Bob directly and, if
   this happens during an active dev session, record it in that session's
   `STATE.md` entry the same way any other finding is recorded, flagged
   clearly as a suspected incident, not a routine bug.
2. **Bob (or whoever Bob delegates) makes the call on:**
   - Whether this is actually a personal-data breach under Art. 4(12), or
     a security issue with no personal-data exposure (still worth fixing,
     but doesn't trigger the notification obligations below).
   - Immediate containment steps (rotate credentials, revoke access,
     isolate the affected container/service) — contain first, investigate
     scope second; don't leave a known hole open while writing up the
     incident.
3. **If personal data of a client's data subjects was involved**, the
   Controller-notification clock starts (AVV § 5.6's "without undue delay,"
   target 48 hours from confirmation — not from initial suspicion, since a
   false-positive notification has its own costs, but don't stretch
   "confirmation" to stall). Notify the specific client Controller(s)
   whose data was affected, not a blanket announcement.
4. **If HELLFIRE's own data (not under any client AVV) was involved** —
   e.g. HELLFIRE's own lead data in gtm-agent's HubSpot — this is
   HELLFIRE acting as controller, and **Art. 33 DSGVO's 72-hour
   notification to the supervisory authority** applies if the breach is
   likely to result in a risk to the rights and freedoms of natural
   persons. This is a real legal obligation with a real deadline — if this
   scenario happens, get counsel involved immediately rather than relying
   on this document to determine whether the 72-hour clock has been met.

## What gets documented per incident

Minimum record, regardless of severity (mirrors Art. 33(5)'s
documentation requirement for controllers, and is good practice for a
processor's own accountability regardless):

- What happened, and how it was detected
- What personal data (categories, approximate number of data subjects) was
  affected, if any
- When it happened, when it was detected, when it was contained
- Who was notified (Controller(s), supervisory authority if applicable),
  and when
- Remediation taken, and what changes (if any) prevent recurrence

Where this record lives today: `STATE.md`, in a dedicated session entry, the
same way every other significant finding in this project is recorded — not
a separate incident-tracking system, since none exists yet and inventing
one before a real incident happens would be premature.

## Open items (flagged, not resolved by this document)

- **External alerting channel** — see above, blocking real automated
  detection.
- **Per-client incident log**, once multiple client AVVs exist — right now
  `STATE.md` is a single shared project log; once there are several live
  client engagements, a breach affecting one client shouldn't require
  digging through an unrelated shared log to reconstruct what was
  disclosed to whom. Revisit when the second real client AVV is signed,
  not before — no need to over-build this ahead of actual need.
- **Formal breach-notification template** (the actual email/letter sent to
  a Controller) doesn't exist yet — worth drafting once the mailbox exists
  and this process has an actual channel to send from.
