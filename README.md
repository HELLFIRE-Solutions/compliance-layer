# compliance-layer

The standard that HELLFIRE modules follow when they contact a person without prior consent, under Art. 6(1)(f) GDPR (DSGVO): what must be recorded, how opt-out is enforced, and when a record must be reviewed again. The repository is documentation only and is **not legal advice**.

## Status

One document, `docs/legitimate-interest.md`, adopted in July 2026. It describes a schema that is implemented in code in [`gtm-agent`](https://github.com/HELLFIRE-Solutions/gtm-agent) (`src/gtm_agent/models.py`), not here.

## What the standard requires

- Every contact made without consent carries a legitimate-interest record. The record states where the lead came from, the concrete fact that establishes a prior connection, the legal basis, a written balancing test, who recorded it and when, an opt-out timestamp, and a review date.
- Once `opt_out_at` is set, the module refuses any further contact with that person. This is enforced in code (`Lead.require_legitimate_interest()` in gtm-agent), not left to a process.
- The record is reused as it is across modules; no module defines its own variant.

## Using it in another module

Import `LegitimateInterestRecord` from gtm-agent instead of redefining it, and call the same guard before every outbound message. `docs/legitimate-interest.md` lists the checks a module must pass.

## Licence

MIT. See `LICENSE`.
