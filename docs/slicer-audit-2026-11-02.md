# Read-only PrusaSlicer profile and start/end audit — 2 November 2026

Status: **observed, not print-tested**. Implements [issue #5](https://github.com/Thetrojantroy/3d-printer-configs/issues/5), after the guide and hardware documentation in issues #2 and #4. This contribution changes documentation only. It reads saved local profiles and the Ender 5 S1 configuration/status; no settings, G-code, heating, movement, restart, flash or deployment was performed.

## Evidence, scope and privacy

Inspected the default Windows roaming PrusaSlicer profile store, installed Creality vendor bundle, saved app selection, historical slicer snapshots and the S1's current main/included Klipper configuration plus Moonraker read-only runtime status. The installed and running slicer reports **2.9.5**; the saved application file also records 2.9.5. No alternate data-directory command-line flag or portable sibling data folder was found for the running slicer. These checks do not inspect unsaved in-memory edits, a loaded project’s per-object overrides, every possible alternate store or another computer.

Found **3 custom printer, 28 custom print and 6 custom filament presets**, with no physical-printer preset files in this store. All 37 custom files have empty `inherits`: they are standalone saved values, not live children of vendor profiles. The vendor bundle contains system presets as a separate layer. Six relevant 0.4/0.5 mm system variants were resolved recursively through their model and nozzle parents; their values must not be confused with custom presets.

Raw copies, profile names/filename mappings, source paths, metadata and hashes remain local outside public Git. Public aliases PR01–PR03, PT01–PT28 and FI01–FI06 refer only to this audit; renaming/sorting profiles later does not preserve these IDs automatically. No raw export, connection setting, personal project name, camera image, log or unsanitized configuration is published.

The saved selection maps to **PR03 + PT16 + FI04**. This is a preference-file observation, not proof of the currently edited scene, nozzle fitted or last printed combination. Earlier stored S1 jobs reported PrusaSlicer 2.9.6 and a different explicitly S1-labelled profile. That job evidence remains historical; this local 2.9.5 store does not contain that custom S1 preset. Its source/export and any 0.4/0.5 S1 custom variants remain missing, rather than reconstructed by guesswork.

## Printer mapping and inheritance

| Alias / layer | Model declaration / nozzle | Firmware and retraction baseline | Mapping and gaps |
|---|---|---|---|
| PR01 custom | Generic ENDER5 / 0.4 mm | Marlin; 0.8 mm retract, 30/30 mm/s retract/unretract, restart extra 0 | Generic Ender-5 family; does not establish whether used for the customized Pro. Model-to-physical-printer mapping needs owner confirmation. |
| PR02 custom | Generic ENDER5 / 0.5 mm | Marlin; 2 mm retract, 30/30 mm/s, extra 0 | Also generic, not explicitly S1/Pro. `printer_variant` still says 0.4 while `nozzle_diameter` says 0.5. Do not use as a proven Klipper S1 profile. |
| PR03 custom | ENDER3S1 / 0.4 mm | Marlin; 2 mm retract, 40/40 mm/s, extra 0 | Fits reported firmware family, but modified hotend/mount geometry is unverified. No custom 0.5 variant found. |
| System Ender-5 S1 0.4 / 0.5 | ENDER5S1, corresponding nozzle | Marlin; 0.8 mm, 30/30 mm/s, extra 0 | Vendor model parent plus nozzle parent; **not Klipper-adapted**. Neither variant's start probes a fresh mesh. |
| System Ender-5 Pro 0.4 / 0.5 | ENDER5PRO, corresponding nozzle | Marlin; 4 mm, 60/40 mm/s, extra 0 | Vendor model plus nozzle parents. Not evidence that the customized Pro uses stock hardware/retraction. |
| System Ender-3 S1 0.4 / 0.5 | ENDER3S1, corresponding nozzle | Marlin; 0.8 mm, 30/30 mm/s, extra 0 | Vendor model plus nozzle parents. Not evidence for measured offsets or modified mount safety. |

All custom printer presets use relative extrusion (M83), slicer-generated retraction (`use_firmware_retraction=0`), zero configured lift and `machine_limits_usage=emit_to_gcode`. Numeric filament overrides replace the corresponding printer value; `nil` leaves that item to the printer preset. No custom preset inherits a vendor update merely because it shares a model name.

Custom `printer_settings_id` metadata does not uniquely distinguish both generic Ender-5 variants, so it is not a reliable machine identity check. Use actual nozzle fields and a confirmed physical-printer mapping; no software declaration measures a nozzle.

## Filament settings and effective retraction

Values below are saved settings, not recommendations. First-layer and subsequent nozzle/bed temperatures are equal within each listed profile. All use 1.75 mm filament and extrusion multiplier 1.0.

| Alias / label category | Nozzle / bed °C | Filament flow cap mm³/s | Retraction overrides | Effect |
|---|---:|---:|---|---|
| FI01 PET-type, label 0.4 | 225 / 75 | 0 (disabled) | Listed length/speed/unretract/extra/lift fields nil | Printer retraction baseline applies |
| FI02 PET-type, label 0.5 | 230 / 75 | 0 (disabled) | Same nil fields | Printer baseline applies; not the historical S1 job's 235°C |
| FI03 PLA, label 0.4 | 210 / 70 | 15 | 4 mm at 80 mm/s, unretract 60 mm/s, extra +0.2 mm | Replaces even PR03's 2 mm / 40 mm/s defaults; requires physical validation |
| FI04 PLA, label 0.4 | 215 / 70 | 15 | Listed fields nil | Printer baseline applies; saved selection uses PR03's 2 mm / 40 mm/s |
| FI05 PLA, label 0.5 | 215 / 70 | 0 (disabled) | 2 mm at 50/50 mm/s, extra +0.1 mm; travel threshold 2 mm | Overrides PR01 as well if paired; label alone does not restrict nozzle |
| FI06 TPU, label 0.4 | 220 / 60 | 11 | Listed fields nil | Printer baseline applies; not an independently tuned flexible-filament result |

Filament compatibility conditions only select a broad Creality vendor-note marker; they do **not** constrain nozzle diameter or a particular machine. A label mentioning a nozzle does not enforce it. All 28 print presets have global volumetric cap zero; therefore FI01/FI02/FI05 combinations have no cap from either of those two settings, while FI03/FI04/FI06 supply the positive filament cap. Validate flow instead of copying these caps to a different hotend/nozzle. Prusa's [volumetric-speed explanation](https://help.prusa3d.com/article/max-volumetric-speed_127176) describes the two limits and zero/disabled behavior.

Filament start/end G-code is comment-only in all six files. No PA/linear-advance command was found there. No nonempty print-preset post-processing or executable custom G-code hook was found in the inspected saved print files. This does not rule out unsaved/project edits or external transformations.

## Print presets, filtering and motion

All 28 have default acceleration **500 mm/s²**, zero first-layer/perimeter/infill/travel acceleration overrides and global max volumetric speed **0**. Zero per-feature overrides are not evidence of zero physical acceleration; final emitted commands must be inspected. Their compatibility conditions broadly accept Ender/CR/Sermoon model strings and compare numeric nozzle diameter. Counts: 13 require 0.4, 7 require 0.5 and 8 require 0.6 mm. No custom printer in this store declares 0.6 mm.

Five labels indicate 0.5 mm but conditions require 0.6 mm (PT03–PT07). This is a saved-data inconsistency, not proof of a physically incorrect nozzle or authorization to change the filter. Other 0.6-filtered profiles are explicitly 0.6-labelled. Compatibility filtering is not a physical safety interlock.

See the [complete sanitized print table](slicer-print-inventory-2026-11-02.md) for every alias, layer/width, nozzle filter and requested speed. Requested travel reaches 300/500 mm/s in some profiles; actual motion depends on the firmware and acceleration, not the label.

| Custom printer | Emitted-setting baseline | Constraint / risk |
|---|---|---|
| PR01 | XY accel 1000; Z accel 1000; XY feed 500; Z feed 12; height 300 | Bed outline spans X 5–230, Y 2.5–222.5; no confirmed mapping to actual Pro or S1 |
| PR02 | XY accel 5000; Z accel 1000; XY feed 500; Z feed 12; height 300 | Outline 229×224; if misused on S1, exceeds its configured 280 mm Z and 222×220 XY limits |
| PR03 | XY accel 500; Z accel 100; XY feed 500; Z feed 10; height 270 | Outline X 5–215, Y 0–220; custom mount/probe geometry remains unmeasured |

Feed units are mm/s and acceleration mm/s². The local generic Ender-5 profiles are not interchangeable with the S1. The current S1 runtime remains max velocity 300, acceleration 5000, Z velocity 5 and Z acceleration 100. Marlin machine-limit emission does not install those values in Klipper. Prior installed-source checks found no compatibility handlers for M201/M203/M205; M204 does change runtime acceleration. A fresh sliced test must show which commands actually occur, including per-feature and ending settings.

## Start/end sequence review

### Custom generic Ender-5 PR01 / PR02 (mapping to Pro unconfirmed)

Start: G90/M83, bed target, nozzle standby 150°C, bed wait, G28, **G29**, final nozzle target/wait, purge, reset E. This preserves their current Marlin-style probing intent. No new soak interval is defined. G29 compatibility with the Pro's actual Marlin build is **unverified**; do not change Pro firmware assumptions based on S1 Klipper behavior.

PR01 purges along X -3 to -2.7, outside its declared positive-X bed outline. PR02 purges at X 0.3. These may be deliberate off-bed purge positions, but actual reachable coordinates and clearance need confirmation. They must not be transplanted to another machine.

End: relative mode, two 2 mm retractions (one combined with Z +0.2), XY +5/+5 wipe, Z +20, absolute mode, home XY, then fan/heaters off and motor disable. This is distinct from the S1 historical bed-lower/park end. The unconditional relative raises/wipe can exceed remaining travel near the top/edge; end homing and shutdown order need an approved clearance review. Do not execute this sequence as an audit test.

### Custom Ender-3 S1 PR03 (Marlin)

Start: relative extrusion/absolute positioning, nozzle standby, bed target, 30-second delay, G28 and **G29 before the bed-temperature wait**, Z50 and XY positioning, final nozzle target, bed/nozzle waits, lower and two purge lines at negative X. Thus probing can occur before the bed has stabilized; the delay does not establish a controlled bed soak. The G28 comment claiming restored leveling is a comment, not verified Marlin behavior. Fresh G29 follows it.

The custom mount and unmeasured nozzle/probe offsets from issue #4 make probe coverage, negative-X purge clearance and Z positions a physical verification gate. End uses conditional Z lifts/present position, followed by heat/fan off and motor disable; it differs from the generic Ender-5 home-at-end sequence. Preserve these printer-specific choices until reviewed, rather than unifying them automatically.

### Vendor variants and historical/current Klipper S1

The six inspected vendor 0.4/0.5 starts share warmup/homing/waits/purge but contain **neither G29 nor BED_MESH_CALIBRATE**. Their existence is not evidence of a safe current profile. Ender-5 S1/Pro vendor ends lower/park the bed; Ender-3 S1 vendor ends conditionally lift/present. Do not replace custom behavior with a vendor preset without reviewing those differences.

The historical S1 jobs inspected in the earlier audit **do** run BED_MESH_CALIBRATE after G28 and a bed wait, before final nozzle heat/purge. Fresh probing for **every S1 print is intentional and must remain**. Those historical start/end definitions cannot be recovered as a present custom profile from this local store; obtain the actual saved S1 export or confirm its source before editing.

Current S1 configuration was read again for this issue. Main/include hashes match the earlier private baseline. It still loads standard Mainsail pause/resume/cancel helpers only: no START_PRINT/PRINT_START, END_PRINT/PRINT_END or G29 wrapper, no PA/shape/motion-limit macro override found. PA is 0, shaping absent, nozzle declared 0.4 and rotation distance 7.5. Runtime was standby with heater targets zero. A generic G29-based profile sent to this S1 would not become fresh Klipper probing automatically.

All three custom printers enable automatic temperature-command emission and also provide explicit temperature commands. That combination alone does not prove duplicate commands: inspect a newly sliced file with the chosen slicer version before concluding which automatic commands were suppressed/emitted. M104 then M109, or M140 then M190, can be an intentional set-and-wait pair. Filament hooks here are comments, and no loaded S1 start macro duplicates probing/heating. New duplicate operations have not been demonstrated by executing a job.

## Priority findings and boundaries

1. **Obtain/identify the real S1 profile.** Do not use a generic Ender-5 or stock Marlin S1 preset as an implicit replacement. Its 0.5 physical-nozzle owner report, 0.4 firmware declaration and historical 0.5 jobs require explicit alignment.
2. **Resolve label/filter contradictions and machine mapping** without silently editing presets. PR02 also has 0.4 variant metadata versus 0.5 nozzle. Nozzle identity cannot be inferred from matching strings.
3. **Treat filament overrides as part of calibration.** In particular FI03's 4 mm / 80 mm/s overrides and restart extras need hardware-specific validation; printer defaults alone are misleading.
4. **Review actual startup/end geometry and thermal order**, especially unmeasured Ender-3 S1 mount offsets and pre-stabilization probing. Confirm the modified Pro's firmware/probe support before adopting G29 assumptions.
5. **Establish flow and motion caps from evidence.** Current 0/11/15 flow limits, requested speeds and acceleration fields are observed, not known-good results.

No print/sensor/nozzle/offset measurements were taken here. Marlin EEPROM settings, full Pro modifications, loaded project overrides, slicer network destinations, any alternate S1 profile source and physical nozzle identities remain outside verified scope. Neither Marlin machine has been contacted. No secret values or public network endpoints are needed to resolve those gaps.

Next work and rollback are explicit in the [slicer change plan](slicer-change-plan.md). See [configuration history](configuration-history.md) and the [filament calibration template](../shared/templates/filament-calibration.md) before changing a profile. All settings in this report remain **observed**, not accepted calibration recommendations.
