# RUN BUNDLE SPEC (Canonical V1)

## Purpose
A Run Bundle is the **minimum replayable unit** for any engine execution under CORE.

A bundle must allow:
- deterministic replay
- schema validation
- hash verification
- provenance inspection
- artifact completeness auditing

## Directory Layout (Canonical)

A single run bundle MUST be materialized as a folder:

`RUN_BUNDLE/<run_id>/`

With required children:

### 1) Definition + Contracts (Required)
- `ENGINE_MANIFEST.json` (copied from engine repo at time of run)
- `INPUT_SCHEMA.json` (copied from engine repo at time of run)
- `OUTPUT_SCHEMA.json` (copied from engine repo at time of run)
- `COUPLING_RULES.json` (copied from engine repo at time of run)
- `SEALING_SPEC.md` (copied from engine repo at time of run)

### 2) Inputs (Required)
- `RUN_INPUT.json`
- `RUN_CONTEXT_STRIPPED.json` *(optional but recommended; see forbidden keys spec)*
- `RUN_PARAMETERS.json` *(optional; if parameters are separated from RUN_INPUT for UI reasons)*

### 3) Outputs (Required)
- `RUN_OUTPUT.json`

### 4) Indexing + Provenance (Required)
- `ARTIFACT_INDEX.json`
- `SHA256SUMS.txt`

### 5) Replay Metadata (Required)
- `RUN_META.json`

## Required File Semantics

### RUN_INPUT.json
- Must be valid JSON
- Must validate against INPUT_SCHEMA.json
- Must NOT contain forbidden context keys after stripping (see FORBIDDEN_CONTEXT_KEYS spec)
- Must be canonicalized prior to hashing and sealing

### RUN_OUTPUT.json
- Must be valid JSON
- Must validate against OUTPUT_SCHEMA.json
- Must include:
  - `inputs_hash` (sha256 over canonicalized RUN_INPUT.json)
  - `outputs_hash` (sha256 over canonicalized RUN_OUTPUT.json or canonical output payload, per sealing spec)
  - `engine.engine_code`
  - `engine.engine_version`
  - `semantics` enum value

### ARTIFACT_INDEX.json
A list of artifacts emitted (including RUN_INPUT/OUTPUT). Required fields:

- `artifact_name`
- `relative_path`
- `sha256`
- `content_type`
- `role` (e.g., CONTRACT, INPUT, OUTPUT, META, INDEX)

### SHA256SUMS.txt
- One line per artifact: `<sha256>  <relative_path>`
- Computed using canonicalization rules for JSON artifacts (see CANONICALIZATION spec)
- Non-JSON artifacts are hashed as raw bytes

### RUN_META.json
Must include:
- `run_id`
- `engine_code`
- `engine_version`
- `sealed_at` (ISO8601 UTC)
- `canonicalization` (must match spec name, e.g. `JCS_RFC8785`)
- `hash_algo` (sha256)
- `core_orchestrator_version`
- `contracts_hashes` (hashes for manifest + schemas + coupling rules + sealing spec)

## Replay Rules (Hard Requirements)

A run is replayable if and only if:
1) All required artifacts exist
2) RUN_INPUT validates against INPUT_SCHEMA
3) RUN_OUTPUT validates against OUTPUT_SCHEMA
4) Hashes match SHA256SUMS
5) Engine contracts hashes match those recorded in RUN_META
6) The engine binary/version used for replay is identical to RUN_META (or explicitly permitted by a forward-compat rule)

## Sealing Rules (Hard Requirements)

CORE seals by:
- copying contracts into the bundle
- stripping forbidden context keys
- canonicalizing JSON artifacts using RFC 8785 (JCS)
- hashing all artifacts and producing SHA256SUMS
- ensuring RUN_OUTPUT includes `inputs_hash` and `outputs_hash`
- rejecting any output that contains forbidden fields or violates schema
