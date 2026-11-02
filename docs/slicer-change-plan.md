# Proposed responsibility split and safe next steps

Status: plan only, based on the [actual saved-profile audit](slicer-audit-2026-11-02.md). No preset, firmware setting, macro or printer operation is changed here.

## Responsibility split

| Responsibility | Keep / proposed owner | Boundary |
|---|---|---|
| Physical nozzle, offsets, board, recovery | Owner-confirmed hardware record | Software cannot detect nozzle size or validate custom mount geometry |
| Mechanical extrusion distance, travel and heater safety limits | Printer firmware | Never use filament flow overrides or larger safety limits to hide a geometry/nozzle mismatch |
| Layer/width, feature speeds, supports and part-specific choices | PrusaSlicer print preset | Inspect compatibility and generated commands for the selected nozzle/machine |
| Temperature, extrusion multiplier, calibrated flow cap and justified retraction override | PrusaSlicer filament preset | Keep scope to material/nozzle/hotend; explicitly resolve nil versus numeric overrides |
| PA | Klipper capability, with one deliberate material-selection path | Currently zero. Tune first; choose either explicit profile start command or parameterized macro, not competing hidden values. Do not send Klipper PA to Marlin machines. |
| Input shaping and validated motion ceiling | Klipper mechanical baseline for S1 | Slicer feature acceleration must be reviewed against the measured ceiling; do not assume the config default cannot be replaced at runtime |
| S1 start/end | Preserve existing behavior in slicer initially | A future START_PRINT/END_PRINT macro can centralize sequencing, but only in its own reviewed/approved change with explicit parameters and no duplicate operations |
| Fresh S1 mesh | Exactly one deliberate fresh probing operation on **every print** after homing and chosen thermal stabilization | Do not replace with a saved-only mesh or overwrite a fresh mesh with an old profile. No adaptive/saved-only change is proposed. |
| Ender-5 Pro / Ender-3 S1 start/end | Their Marlin-aware printer presets | Preserve distinct park/lift/home behavior; confirm actual firmware and custom geometry. No Klipper macro assumptions before migration. |

## Stages, tests and rollback

| Stage | Test before acceptance | Owner / physical involvement | Rollback |
|---|---|---|---|
| 1. Confirm source and nozzle | Locate the missing custom Klipper S1 preset; compare actual nozzle, firmware declaration and selected preset. Map generic PR01/PR02 to physical machines explicitly. | Owner confirms nozzle and which printer uses each profile. No motion needed. | No changes yet; retain current private source snapshot |
| 2. Prepare isolated profile corrections | Proposed diff only: Klipper flavour for the correct S1 profile; resolve PR02 variant metadata and PT03–PT07 label/filter contradictions without guessing intent. Check 0.4/0.5 selection/filter cases and incorrect/missing metadata warnings. | Owner reviews intended mapping and preserved production presets. | Discard experimental copy; reselect original profiles |
| 3. Offline slice review | Use a simple synthetic model, no automatic upload. Inspect emitted start/end, bounds, relative/absolute E, retraction overrides, temperature set/wait commands and all M204/legacy/PA/mesh commands. Confirm exactly one fresh S1 probe sequence per print. Include no user project metadata in public evidence. | Review expected clearances; this stage does not authorize sending or printing the file. | Delete/reject test output; original profiles remain unchanged |
| 4. Geometry and thermal validation | After separate approval, verify custom-mount offsets/probe coverage and mechanical travel, thermal ordering and safe end clearance. Do not execute a whole unknown end macro just to test it. | Owner present for approved movement/heating; measure Ender-3 S1 nozzle/probe geometry. Pro inventory/firmware first. | Stop the test safely; restore reviewed original preset/config from private baseline in an agreed idle window |
| 5. Calibrate systematically | Follow mechanics/extrusion, thermal/PID, bed/first layer, material temperature/flow, shaping, PA, retraction and measured acceleration/flow ceiling stages. Ensure tower parameters are not reset by later slicer commands. | Measurements, sensor mounting or prints and user assessment as specified by the [roadmap](calibration-roadmap.md). S1 gear change requires a fresh extrusion baseline after installation. | Revert only the tested candidate; retain accepted baseline and private rollback copy |
| 6. Regression and release | Slice/review again, then obtain approval for representative prints; verify first layer, corners, seams, fit, temps, safe end and balanced quality/speed. Record actual selected profile hashes and acceptance. | Owner assesses and accepts physical result. | Reselect original known production preset and restore only authorized config changes; no automatic flashing/restart |

Optional macro centralization comes **after** a validated slicer baseline, not as an untested fix. A future macro PR must include argument defaults, exact operation ownership, duplicate detection, every-print fresh probing, a config backup and an approved save/restart window. Keep Marlin start/end separate. Adding macros or activating new motion/heating routines is not authorized by this documentation plan.

## Baseline preservation and status

The audit copied saved custom/vendor profiles and the application selection file locally, retained the two historical snapshots, and recorded source hashes and current S1 config/runtime privately. Before any future edit, close or otherwise coordinate with the running slicer to avoid concurrent saves, make a new dated private backup, and compare hashes with this audit. Do not blindly restore the entire application directory or its connection credentials merely to revert a single preset.

No proposed preset is known-good until its tests and physical acceptance are documented. Use the [filament record](../shared/templates/filament-calibration.md) per nozzle/material/hotend and the [configuration history](configuration-history.md) for each accepted or rejected candidate. Public exports require a separate sanitization review.
