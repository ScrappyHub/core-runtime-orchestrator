# CORE Runtime Orchestrator (Canonical V1)

This repository defines the **CORE-only orchestration layer** responsible for:

- Validating engine manifests and schemas
- Enforcing coupling rules (CORE-mediated only)
- Stripping forbidden context keys prior to sealing
- Canonicalizing JSON via RFC 8785 (JCS)
- Hashing via sha256
- Producing sealed, replayable **Run Bundles**

## Non-Negotiables

- Engines are **headless** and **network-isolated**.
- Engines are **not** user-aware, tier-aware, role-aware, or governance-aware.
- Engines never accept peer delivery.
- **CORE is the only delivery surface** for inputs to engines.
- **CORE is the only enforcement surface** for coupling policy.
- Run Bundles are the unit of replay, verification, and audit.

## Specs

- `spec/RUN_BUNDLE_SPEC.md`
- `spec/CANONICALIZATION.md`
- `spec/COUPLING_ENFORCEMENT.md`
- `spec/FORBIDDEN_CONTEXT_KEYS.md`
- `spec/UNITS_AND_CONVERSIONS.md`
- `spec/PHYSICS_CAPABILITY_MATRIX.md`
