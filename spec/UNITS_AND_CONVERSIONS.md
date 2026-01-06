# UNITS AND CONVERSIONS (Canonical V1)

## Purpose
Ensure outputs are physically interpretable and comparable across engines and vertical lenses.

## Canonical Unit System
Canonical units are SI unless explicitly stated otherwise.

### Required Unit Tagging
Any numeric field that represents a physical quantity MUST be accompanied by:
- unit (string)
- quantity (string)

Accepted pattern (recommended):
- `<field>` numeric value
- `<field>_unit` unit string

Example:
- `wind_speed: 12.3`
- `wind_speed_unit: "m/s"`

Alternative (structured):
- `wind_speed: { "value": 12.3, "unit": "m/s" }`

Engines must be consistent within a repo.

## Canonical Conversions

### Speed
- m/s is canonical
- mph and kph are display optional

Conversions:
- mph → m/s: `mps = mph * 0.44704`
- kph → m/s: `mps = kph / 3.6`
- m/s → mph: `mph = mps / 0.44704`
- m/s → kph: `kph = mps * 3.6`

### Distance
- meters is canonical
- feet/miles display optional

Conversions:
- ft → m: `m = ft * 0.3048`
- mi → m: `m = mi * 1609.344`
- m → ft: `ft = m / 0.3048`
- m → mi: `mi = m / 1609.344`

### Mass
- kg canonical
- lb display optional
- lb → kg: `kg = lb * 0.45359237`
- kg → lb: `lb = kg / 0.45359237`

### Pressure
- Pa canonical
- kPa, bar, psi display optional
- psi → Pa: `Pa = psi * 6894.757293168`

### Temperature
- Kelvin canonical internally
- Celsius display common
- Fahrenheit display optional

Conversions:
- C → K: `K = C + 273.15`
- K → C: `C = K - 273.15`
- F → C: `C = (F - 32) * 5/9`
- C → F: `F = C * 9/5 + 32`

## Run Bundle Requirement
RUN_OUTPUT must include unit tags for any physical quantity surfaced.

Vertical lens repos may render in preferred display units, but sealed outputs remain canonical.
