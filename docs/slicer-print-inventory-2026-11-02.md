# Sanitized custom print-preset inventory — 2 November 2026

Status: observed only; no slicing or printing performed. Public aliases map to private source files retained locally. This table publishes selected numeric settings, not raw filenames or exports. See the [main audit](slicer-audit-2026-11-02.md) for scope and override interpretation.

All entries have empty inherits, default acceleration 500 mm/s², global volumetric cap 0 and zero first-layer/perimeter/infill/travel acceleration override fields. Speed columns are requested values in mm/s. Width is the default/perimeter width; first-layer width is listed separately. A label hint is not a physical nozzle measurement or an enforced filter.

| ID | Label category / nozzle hint | Condition nozzle mm | Layer / first layer mm | Width / first width mm | Perimeter / external / infill mm/s | Travel mm/s |
|---|---|---:|---|---|---|---:|
| PT01 | PLA / 0.4 | 0.4 | 0.3 / 0.2 | 0.5 / 0.5 | 70 / 70 / 70 | 150 |
| PT02 | PETG / 0.5 | 0.5 | 0.4 / 0.2 | 0.6 / 0.6 | 40 / 40 / 50 | 150 |
| PT03 | PETG / 0.5 | 0.6 | 0.3 / 0.3 | 0.5 / 0.5 | 50 / 50 / 50 | 150 |
| PT04 | PETG / 0.5 | 0.6 | 0.4 / 0.3 | 0.8 / 0.8 | 50 / 50 / 50 | 150 |
| PT05 | PLA / 0.5 | 0.6 | 0.2 / 0.2 | 0.5 / 0.5 | 70 / 70 / 70 | 300 |
| PT06 | PLA / 0.5 | 0.6 | 0.3 / 0.3 | 0.5 / 0.5 | 70 / 70 / 70 | 500 |
| PT07 | PLA / 0.5 | 0.6 | 0.2 / 0.2 | 0.5 / 0.5 | 30 / 30 / 30 | 300 |
| PT08 | PETG / 0.6 | 0.6 | 0.4 / 0.3 | 0.8 / 0.8 | 50 / 50 / 50 | 150 |
| PT09 | PETG / 0.4 | 0.4 | 0.2 / 0.2 | 0.4 / 0.4 | 60 / 60 / 60 | 150 |
| PT10 | PETG / 0.4 | 0.4 | 0.3 / 0.3 | 0.6 / 0.6 | 60 / 60 / 70 | 300 |
| PT11 | PETG / 0.5 | 0.5 | 0.2 / 0.2 | 0.4 / 0.6 | 70 / 70 / 70 | 150 |
| PT12 | PETG / 0.5 | 0.5 | 0.3 / 0.3 | 0.6 / 0.6 | 70 / 70 / 70 | 150 |
| PT13 | PETG / 0.5 | 0.5 | 0.4 / 0.3 | 0.8 / 0.8 | 70 / 70 / 70 | 150 |
| PT14 | PLA / 0.6 | 0.6 | 0.3 / 0.3 | 0.5 / 0.5 | 70 / 70 / 70 | 150 |
| PT15 | PLA / 0.6 | 0.6 | 0.3 / 0.3 | 0.5 / 0.5 | 70 / 70 / 70 | 300 |
| PT16 | PLA / 0.4 | 0.4 | 0.2 / 0.2 | 0.4 / 0.4 | 70 / 70 / 70 | 150 |
| PT17 | PLA / 0.4 | 0.4 | 0.3 / 0.3 | 0.6 / 0.6 | 70 / 70 / 70 | 150 |
| PT18 | PLA / 0.4 | 0.4 | 0.3 / 0.3 | 0.5 / 0.5 | 70 / 70 / 70 | 150 |
| PT19 | PLA / 0.5 | 0.5 | 0.2 / 0.2 | 0.55 / 0.52 | 70 / 70 / 70 | 150 |
| PT20 | PLA / 0.5 | 0.5 | 0.3 / 0.3 | 0.6 / 0.6 | 70 / 70 / 70 | 150 |
| PT21 | PLA / 0.5 | 0.5 | 0.4 / 0.3 | 0.8 / 0.8 | 80 / 80 / 80 | 150 |
| PT22 | PLA / not stated | 0.4 | 0.2 / 0.2 | 0.4 / 0.4 | 70 / 70 / 70 | 150 |
| PT23 | PLA / not stated | 0.4 | 0.2 / 0.2 | 0.5 / 0.5 | 30 / 30 / 30 | 150 |
| PT24 | PLA / not stated | 0.4 | 0.2 / 0.2 | 0.4 / 0.4 | 60 / 60 / 60 | 150 |
| PT25 | PLA / not stated | 0.4 | 0.3 / 0.3 | 0.4 / 0.4 | 70 / 70 / 70 | 150 |
| PT26 | TPU / 0.4 | 0.4 | 0.2 / 0.2 | 0.4 / 0.4 | 50 / 40 / 60 | 150 |
| PT27 | TPU / 0.4 | 0.4 | 0.2 / 0.2 | 0.4 / 0.4 | 50 / 40 / 60 | 150 |
| PT28 | TPU / 0.4 | 0.4 | 0.3 / 0.3 | 0.6 / 0.6 | 50 / 50 / 50 | 150 |

PT03–PT07: label hints 0.5 but condition requires 0.6. PT08/PT14/PT15 are 0.6-labelled and 0.6-filtered. No custom printer in this store declares 0.6. Conditions also require a broadly matching Ender/CR/Sermoon model string; actual UI filtering is not a physical interlock.
The saved preference selects PT16 with PR03 and FI04, but unsaved project changes and per-object overrides were not inspected.
