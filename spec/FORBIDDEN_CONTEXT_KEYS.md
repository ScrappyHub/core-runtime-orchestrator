# FORBIDDEN CONTEXT KEYS (Canonical V1)

## Purpose
Engines must never see or seal user/governance/billing signals.

Schemas may include metadata:
- `context.x_forbidden_fields`

CORE must enforce by stripping prior to sealing.

## Forbidden Key Categories (Non-Exhaustive)

### Identity / User
- user_email
- user_id
- username
- phone
- oauth_subject
- device_id

### Governance / Access
- role
- roles
- permissions
- feature_flags
- admin
- account_status

### Billing / Monetization
- billing_tier
- subscription_status
- price_id
- invoice_id
- payment_state

## Enforcement (Hard Requirement)

Before sealing RUN_INPUT.json:
1) CORE reads INPUT_SCHEMA.json
2) If `context.x_forbidden_fields` exists, CORE strips those keys from context
3) CORE additionally strips any keys matching the above forbidden categories
4) CORE writes:
   - `RUN_CONTEXT_STRIPPED.json` (optional but recommended)
   - final sealed `RUN_INPUT.json` without forbidden keys
5) Hashing and sealing occur only after stripping

Engines must treat `context` as non-authoritative always.
