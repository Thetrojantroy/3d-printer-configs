# Ender 3 S1 slicer inventory

Status: **observed, not tested**, 2 November 2026. Source: actual saved local PrusaSlicer 2.9.5 files, not only earlier job metadata. See the [full audit](../../docs/slicer-audit-2026-11-02.md) and [print-preset table](../../docs/slicer-print-inventory-2026-11-02.md). Raw exports/names and hashes remain private.

| Layer | Mapping | Observation |
|---|---|---|
| Custom printer PR03 | ENDER3S1, 0.4 nozzle, Marlin | Standalone; 2 mm retract at 40/40 mm/s, restart extra 0, no firmware retraction |
| System variants | Ender-3 S1 0.4 and 0.5 | Installed vendor definitions; 0.8 mm retract at 30/30 mm/s, not the same as PR03 |
| Saved app selection | PR03 + PT16 + FI04 | 0.2 mm layer / 0.4 width; PLA 215°C / bed 70°C; multiplier 1, flow cap 15; filament retraction fields nil, so PR03 baseline applies |
| Custom 0.5 printer variant | Not found for this model | A system 0.5 variant exists but is not proof of current hardware or selected configuration |

Selection-file state does not prove the active project's unsaved settings or a physical print. Broad filament compatibility allows numeric overrides to replace PR03 defaults: FI03 would use 4 mm at 80/60 mm/s and extra +0.2; FI05 uses 2 mm at 50/50 and +0.1. Do not judge retraction solely from the printer tab.

PR03 starts with nozzle standby, bed target, 30-second delay, G28 and G29 **before** bed wait, then final heating/waits and negative-X purge lines. Its end conditionally lifts/presents and shuts heaters/fan off. Preserve this Marlin-specific behavior pending review; verify thermal repeatability and purge/probe clearance with the custom mount. Approximate-stock CR Touch/nozzle offsets remain unmeasured per [issue #4](https://github.com/Thetrojantroy/3d-printer-configs/issues/4).

No presets, offsets or firmware were changed. Follow the [proposed staged plan](../../docs/slicer-change-plan.md) with separate approval for physical tests; retain originals and record accepted filament tests with the [template](../../shared/templates/filament-calibration.md).
