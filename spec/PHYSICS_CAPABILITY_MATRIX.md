# PHYSICS CAPABILITY MATRIX (Canonical V1)

## Purpose
Define a global capability inventory that CORE can:
- advertise in UI
- gate by lane policy
- validate engine readiness
- support replay completeness audits

This matrix is **platform-level**. Engines declare support in their own repo under:
`/CAPABILITIES/PHYSICS_CAPABILITIES.md`

## Canonical Controls & Scaling (Global)

### Distance Scaling Factors (Required Control)
All engines that render or evaluate geometry-dependent fields must support canonical scale factors:

- 0.25
- 0.50
- 0.75
- 1.00

Represent as:
- `distance_scale_factor` ∈ {0.25, 0.5, 0.75, 1.0}

### Speed Domains (Canonical)
Required canonical unit: m/s (display mph/kph allowed)

## Capability List (Global)

Each capability includes:
- Capability Key
- Description
- Canonical Units
- Expected Engine Coverage

### CAP_WIND_SPEED
Wind speed evaluation / tagging
- Units: m/s (display mph/kph)
- Expected: ATMOS, SIGNAL, RGSR (correlation only)

### CAP_MOVING_OBJECT_SPEED
Speed of moving objects through medium (air/water/solid proxy)
- Units: m/s
- Expected: ATMOS (air), HYDRA (water), KINETIC (solid), SIGNAL (transform), RGSR (correlation)

### CAP_DISTANCE_SCALING
Distance scaling factor control
- Units: dimensionless
- Expected: ALL engines that evaluate geometry-dependent fields

### CAP_TSUNAMI_WAVE_DYNAMICS
Wave energy / propagation proxy in water
- Units: m/s, Pa, m
- Expected: HYDRA (+ SIGNAL transforms), RGSR (correlation only)

### CAP_HURRICANE_COUPLING
Atmospheric + pressure + humidity coupling signatures
- Units: m/s, Pa, kg/kg (humidity ratio) or % (display)
- Expected: ATMOS (+ SIGNAL), RGSR (correlation only)

### CAP_MUDSLIDE_FLOW
Dense flow / debris movement proxy
- Units: m/s, Pa
- Expected: HYDRA + LITHOS (+ SIGNAL), RGSR correlation

### CAP_AVALANCHE_FLOW
Granular flow proxy
- Units: m/s
- Expected: HYDRA (as granular approximation if defined), LITHOS/ KINETIC if modeled; otherwise declared NOT_SUPPORTED per engine

### CAP_PROJECTILE_DYNAMICS
Ballistic / trajectory dynamics (non-weapon intent; purely kinematics)
- Units: m/s, m, rad
- Expected: KINETIC (truth), SIGNAL transforms; RGSR correlation
- Note: classification (“missile/rocket”) is lens-level, not engine output

### CAP_SUBMERGED_OBJECT_SIGNATURES
Acoustic / pressure / turbulence signature patterns in water
- Units: Pa, m/s
- Expected: ARES + HYDRA + SIGNAL; RGSR correlation

### CAP_STRUCTURAL_FAILURE_SIGNATURES
Stress/strain resonance signatures and deformation
- Units: Pa, m, dimensionless strain
- Expected: KINETIC (+ SIGNAL), RGSR correlation

### CAP_EM_FIELD_COUPLING
Electromagnetic field strength / induction coupling
- Units: V/m, T, A/m
- Expected: MAGNETAR (+ SIGNAL), RGSR correlation

### CAP_THERMAL_GRADIENTS
Heat flow / gradients / thermal coupling
- Units: K, W/m^2
- Expected: THERMOS (+ SIGNAL), RGSR correlation

### CAP_LAYERED_MEDIA_RESPONSE
Layered subsurface/stack response (impedance / conductivity / absorption)
- Units: domain-specific; must unit-tag
- Expected: LITHOS (+ ARES/HYDRA/MAGNETAR/THERMOS where coupled), RGSR correlation

### CAP_CRYSTAL_RESONANCE
Lattice/phonon/piezoelectric resonance patterns
- Units: Hz, V, dimensionless coupling coefficients
- Expected: CRYSTAL (+ SIGNAL), RGSR correlation

### CAP_SIGNAL_TRANSFORMS
FFT, cross-correlation, coherence, anomaly scoring
- Units: Hz (spectra axes), dimensionless scores must be labeled
- Expected: SIGNAL (truth-adjacent transform)

### CAP_FUSION_CORRELATION
Cross-engine relationship mapping (no causation)
- Units: dimensionless (must be labeled)
- Expected: RGSR

## Classification Policy Note (Hard Rule)
Labels such as:
- “missile”
- “rocket”
- “submarine”
- “tsunami”
- “hurricane”
are **vertical lens interpretations** gated by CORE lane policy.
Engines emit raw truth or transforms only.
RGSR emits correlation-only structures only.
