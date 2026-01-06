# COUPLING ENFORCEMENT (Canonical V1)

## Purpose
Define the only valid coupling model:

✅ CORE-mediated coupling  
❌ Direct engine-to-engine delivery  
❌ Peer delivery  
❌ Engine network access

## Canonical Model

- Engines accept inputs **only from CORE**.
- Engines never accept input directly from another engine.
- Engines never fetch upstream artifacts.
- COUPLING_RULES.json is declarative documentation + gating rules for CORE.
- CORE is the coupling enforcement engine.

## Required COUPLING_RULES Fields

Each engine must include in COUPLING_RULES.json:

- `"delivery_model": "CORE_ONLY"`
- `"core_mediated_coupling": true`
- `"direct_engine_to_engine_calls": false`

COUPLING_RULES may additionally describe:
- allowed upstream artifact sources (by engine_code)
- allowed downstream consumers (by engine_code)

But these are interpreted ONLY by CORE.

## Enforcement Rules (Hard Requirements)

CORE must reject any run request if:
- the requested upstream artifacts violate the target engine's COUPLING_RULES
- upstream artifacts were not sealed by CORE
- upstream artifacts do not match expected hashes
- the engine attempts to access network resources (if detected)
- the engine output includes prohibited fields

## Replay
Replay uses the same coupling rules:
- upstream artifacts must be present in the run bundle
- RGSR must include an input_artifact_index (hash list) referencing upstream bundles/artifacts
