# CANONICALIZATION (RFC 8785 JCS + sha256) — Canonical V1

## Purpose
Prevent hash drift caused by key ordering, whitespace, float formatting, or serializer differences.

## Canonical JSON Scheme
All JSON artifacts intended for hashing MUST be canonicalized using:

**RFC 8785 — JSON Canonicalization Scheme (JCS)**

This spec name is used in manifests and run metadata as:

`JCS_RFC8785`

## Hash Algorithm
- sha256 over canonicalized bytes (UTF-8)

## Rules (Hard Requirements)
1) JSON must be valid UTF-8.
2) Canonicalization MUST follow RFC 8785 exactly.
3) After canonicalization, sha256 is computed over the canonical byte sequence.
4) Engines must not emit nondeterministic numeric formatting.
5) CORE must reject any artifact that cannot be canonicalized.

## What Gets Canonicalized
Canonicalize before hashing:
- RUN_INPUT.json
- RUN_OUTPUT.json
- ENGINE_MANIFEST.json
- INPUT_SCHEMA.json
- OUTPUT_SCHEMA.json
- COUPLING_RULES.json
- ARTIFACT_INDEX.json
- RUN_META.json

Non-JSON artifacts are hashed as raw bytes.

## Recorded Metadata
RUN_META must record:
- `canonicalization: "JCS_RFC8785"`
- `hash_algo: "sha256"`
