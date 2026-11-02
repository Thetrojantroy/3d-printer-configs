# Calibration history

## 2026-11-02: prior read-only baseline

Status: **observed, not calibrated**. Source: the existing local Pi-Camera-Fix workspace's `calibration-baseline/CALIBRATION-AUDIT.md` and `gcode-scan-summary.md`, checked against local configuration copies, runtime snapshot and metadata for this contribution. Original files remain private and are not committed here. No new SSH session or printer test was performed for issue #1.

| Area | Prior observation | Meaning / next step |
|---|---|---|
| Extruder | rotation_distance 7.5, 16 microsteps | Matches installed stock reference; measured travel still unverified |
| PA | Effective 0.0; smooth time 0.04 s | Disabled; tuning pending |
| Input shaping | No shaper or measurement configuration | Missing in observed setup; sensor availability TBD |
| Motion | XY velocity 300 mm/s, acceleration 5000 mm/s², corner velocity 5 mm/s; Z 5 mm/s and 100 mm/s² | Configured maximums, not demonstrated print capabilities |
| Hotend PID | 20.749 / 1.064 / 101.153 | Matches stock reference; verify at intended temperatures |
| Bed PID | 66.566 / 0.958 / 1155.761 | Matches stock reference; verify at intended temperatures |
| Probe | Saved Z offset 3.280, four-sample averaging | Configured; do not copy to another machine or assume current accuracy |
| Mesh | Saved 4×4, historical point spread about 0.254 mm | Old measurement, not current bed-flatness proof; fresh probing is intentional for every print |
| Axis twist | Calibration section present, correction arrays empty | Not calibrated; investigate only if justified |
| Macros | Standard Mainsail pause/resume/cancel helpers; no custom print-start/end macro loaded | Start/end control resides in audited slicer jobs |
| Runtime | Idle snapshot: PA zero, speed/flow factors 1, no mesh active | Does not establish state during old prints; jobs probe on start |

Scan scope: all 31 stored `.gcode` files found at the prior audit. All contained fresh probing and acceleration overrides. No active PA, shaper, direct velocity-limit, extruder-distance or G-code-offset override matched that scan. See slicer inventory for scope limits. Raw job names, diagnostic data and identifying paths are intentionally omitted.

## Owner clarification for repository design

Issue #1 states the usual physical nozzle is 0.5 mm, existing PrusaSlicer presets should be reused, and fresh probing on every print is intentional. This updates the roadmap's assumptions; it does **not** change the prior observed firmware declaration of 0.4 mm or establish which nozzle is fitted today.

## Next results

Read-only issue #5 recheck, 2 November 2026: current config/include hashes match the prior baseline; no settings changed. The [local slicer audit](../../docs/slicer-audit-2026-11-02.md) found a missing custom S1 mapping and filament/filter conflicts, not new calibration results. All audited presets remain observed; none is newly tested/accepted.

TBD: no new physical calibration result is claimed. Use the [record template](../../shared/templates/calibration-record.md) for the next approved stage, beginning with actual nozzle/profile alignment.
