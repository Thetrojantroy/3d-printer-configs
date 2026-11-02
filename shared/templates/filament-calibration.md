# Filament calibration and profile-status record

Status: template / no test performed. Keep raw exports, sensitive paths and spool/account identifiers private.

| Field | Record |
|---|---|
| Printer / physical nozzle confirmation / hotend / extruder state | TBD |
| Date, material, non-identifying spool alias, drying/condition | TBD |
| Slicer version and printer/print/filament public aliases | TBD |
| Private original and candidate preset hashes | TBD |
| Profile status | Observed / experimental / rejected / tested-and-accepted: TBD |
| Printer retraction baseline | Length, retract/unretract speeds, restart extra, lift, travel threshold: TBD |
| Filament overrides | For every item record **nil/inherit** or numeric override: TBD |
| Effective combined retraction and restart extra | TBD |
| Nozzle/bed temperatures, fan, soak and environment | TBD |
| Measured extruder-distance verification | TBD; do not use multiplier to conceal incorrect travel |
| Flow multiplier and volumetric cap | Baseline, tested candidate, measured failure/quality boundary and conservative accepted limit: TBD |
| PA / shaping state and source of commands | TBD; identify every possible override |
| Test method / sliced-file hash / offline G-code checks | TBD |
| Separate authorization for heating/motion/printing | TBD |
| Physical measurements, photos and owner quality assessment | TBD |
| Acceptance criteria and actual result | TBD |
| Before / candidate / accepted values | TBD |
| Dependent rechecks after hardware/material changes | TBD |
| Rollback preset/config reference and review PR | TBD |

Do not label a profile known-good because it was present in the slicer, matched an old snapshot, sliced successfully or produced a camera image. Record an accepted physical test for the actual combination. Preserve fresh probing on every Ender 5 S1 print and printer-specific start/end behavior during experiments.
