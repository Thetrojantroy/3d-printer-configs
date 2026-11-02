# Design decisions

Status: accepted project direction from issue #1; operational implementation remains proposed unless a test record says otherwise.

| Decision | Reason and boundary |
|---|---|
| One mainline, three printer directories | Share documentation and review conventions without treating different hardware as interchangeable. No permanent branch per printer. |
| Balance quality and speed | Tune for repeatable parts, reliable first layers and usable surfaces, rather than maximum advertised motion rates. |
| Fresh Ender 5 S1 mesh on every print | Intentional owner workflow, supported by all 31 stored jobs in the prior audit. Preserve homing followed by fresh probing before printing. Do not substitute an old saved mesh or load one over a fresh result. |
| Preserve printer-specific start/end behavior | Compare existing heating, homing, probing, purge, park and shutdown behavior before any refactor. A shared template must not silently replace machine-specific procedures. |
| Reuse existing PrusaSlicer presets | Owner reports existing printer/nozzle/filament presets. Inventory/export/review them before creating alternatives. Old G-code metadata is evidence about that job, not today's active preset. |
| Physical nozzle must be confirmed | Owner reports the Ender 5 S1 usually uses 0.5 mm. Prior audit saw Klipper declaring 0.4 mm and a sliced job declaring 0.5 mm. Neither software setting proves the nozzle fitted today. |
| Treat PA/shaping as pending calibration | Prior audit observed pressure advance zero and no input shaping configuration. Do not invent tuned values. |

## Proposed nozzle/profile consistency check

Status: design only; no checker or macro is installed by this contribution.

Inputs should distinguish (a) an owner-confirmed physical nozzle diameter and confirmation date, (b) the selected printer/nozzle preset, (c) the sliced file's declared nozzle, and (d) Klipper's configured nozzle diameter. A missing input produces **unknown**, not **match**.

Compare declared inputs before printing. On disagreement, show which values disagree and ask the owner to inspect the nozzle and selected preset. Never claim that a matching software value proves the physical nozzle; re-confirm after a nozzle change. Re-export/re-slice after resolving a mismatch. Never rewrite an existing production job automatically.

The check should be read-only and issue a visible warning or require explicit acknowledgement at a defined preflight step. It must not silently change nozzle diameter, maximum extrusion cross-section, heater limits, extruder distance, flow, PA or other safety parameters. A software/configuration correction requires its own reviewed change and authorization. Failure to read metadata must not look like a passed check.

Acceptance cases for a future implementation: confirmed match; declared mismatch; no physical confirmation; missing/malformed metadata; multiple nozzle declarations; stale confirmation after nozzle swap; current preset differs from an older sliced file. Tests must use synthetic, public-safe data.
