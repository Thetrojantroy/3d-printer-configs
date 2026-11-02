# Ender 5 S1 profile source and nozzle mismatch investigation

Date: 2 November 2026. Status: **read-only findings, not calibrated or print-tested**. No profile was imported/saved, no model sliced, no G-code sent, no printer setting changed, no heating/movement/restart initiated and no PR merged.

## Result

An accessible recent **3MF project contains an explicit Ender-5 S1 / 0.5 mm configuration**. The default saved custom-preset directory still does not contain a named custom S1 printer preset. These are separate storage layers, so absence from the preset directory does not mean the S1 settings are lost.

The project's 0.5 mm declaration and all 31 previously audited job nozzle declarations disagree with the unchanged current Klipper declaration of **0.4 mm**. The owner's statement that the S1 usually uses 0.5 mm supports that intended setup, but none of these software records measures the nozzle fitted today. A stale firmware declaration is a hypothesis, not a physically verified root cause. No automatic correction is justified.

## Sources and read-only method

- Rechecked current S1 main/included configuration hashes and read-only Moonraker status through existing SSH. Both source hashes match the earlier private baseline. Printer was standby; heater targets were zero. No G-code API call was made.
- Checked the actual Windows PrusaSlicer saved profile store against the earlier 37-file baseline; no saved custom file changed. Prior audit of the running/installed application found version 2.9.5 and no alternate data-directory switch or portable sibling data folder.
- Followed the saved application's recent-project references: nine entries, six accessible and three unavailable. Read accessible ZIP entries with a read-only archive handle; no project was opened or resaved through the slicer. Inspected embedded slicer configuration and application-version metadata, not published models or scene content.
- Used project aliases **J01–J09** tied to private source paths/hashes. J01 is the accessible project with explicit ENDER5S1 metadata; these aliases are unrelated to the PR/PT/FI preset aliases in the [earlier audit PR #8](https://github.com/Thetrojantroy/3d-printer-configs/pull/8).
- Compared J01's start/end strings and selected numeric fields with previously captured S1 job metadata. No fresh job scan or physical test is implied. Recent config/output directories had no top-level INI export files; this is a bounded check, not a whole-disk absence claim.
- Confirmed all six inspected project files and all 37 custom source presets retained their pre-inspection hashes. Raw embedded configurations, recent paths, original project/preset names and evidence remain private outside the public checkout.

Prusa documents that [saved 3MF projects contain objects and settings](https://help.prusa3d.com/article/saving-projects-as-3mf_1773). Reading a project's settings does not establish the slicer's currently selected in-memory configuration or prove the exact source of a previous job.

## Source layers and what they prove

| Layer | Observed evidence | Interpretation / limit |
|---|---|---|
| Physically fitted nozzle | Not measured in this investigation | Remains unknown; owner reports usual 0.5 mm, not fresh confirmation |
| Current S1 Klipper | nozzle_diameter 0.4; rotation_distance 7.5; effective max_extrude_cross_section 0.64 mm² | Firmware declarations/defaults, not a nozzle measurement; unchanged from baseline |
| Project J01 | printer_model ENDER5S1; printer_variant 0.5; nozzle_diameter 0.5 | A recoverable project-embedded S1 configuration, not an installed custom preset or accepted calibration |
| Earlier job metadata | All 31 captured nozzle declarations are 0.5; temperature 235°C; retraction 1 mm; restart extra +0.2 mm | Strong evidence of historical slicing choices; not proof of the physical nozzle or that J01 produced every job |
| Saved custom printer PR02 | Generic ENDER5, nozzle 0.5 but variant metadata 0.4 | Separate metadata mismatch; not the same as J01, whose nozzle and variant both say 0.5 |
| Saved print PT03–PT07 | Labels suggest 0.5 while compatibility conditions require 0.6 | Separate label/filter inconsistency; no filter was changed |
| Installed vendor S1 0.4/0.5 | Marlin-oriented starts without fresh probing | Available system definitions; do not replace the project configuration or intentional fresh-mesh workflow blindly |

The earlier audit correctly described the saved preset directory but did not inspect these recent project archives. Its missing-source conclusion is now narrowed: **the named saved custom S1 preset is still absent, while project-embedded S1 settings are available**. PR #8 remains unmerged; this follow-up documents the additional source without rewriting its historical scope.

## Project J01: recovered S1 settings

These are observations, not a ready-to-deploy preset. Only selected, sanitized values are published.

| Item | Project J01 | Current S1 / consequence |
|---|---|---|
| Model, nozzle variant, declared diameter | ENDER5S1 / 0.5 / 0.5 mm | Firmware declares 0.4; physical confirmation still needed |
| G-code flavour / machine-limit use | Marlin / emit_to_gcode | S1 runs Klipper; legacy machine-limit emission needs an offline output review, not blind transfer |
| Bed outline | X 0–224, Y 0–224 mm | Klipper axis limits are X 0–222, Y 0–220; a future slice can place moves beyond configured travel |
| Maximum print height | 270 mm | Klipper Z maximum is 280; different, more restrictive project setting, not itself evidence of unsafe extra travel |
| Layer height / default width | 0.4 / 0.8 mm | Intentional-looking production choices, not a controlled calibration baseline or evidence of nozzle size |
| Nozzle / bed temperature | 235 / 75°C | Matches the captured historical job settings; not a material recommendation |
| Extrusion multiplier | 1.0 | No physical flow calibration established |
| Retraction | 1 mm, retract/unretract 30/30 mm/s, restart extra +0.2 mm | Listed filament length/speed/unretract/extra overrides are nil, so this project baseline applies |
| Flow limits | Print and filament maximum volumetric speeds both 0 | No cap from these two settings; measure flow before proposing a working cap |
| Default print acceleration | 500 mm/s² | Actual generated M204 sequence must be checked |
| Machine fields | XY acceleration 5000, Z acceleration 1000; XY feed 500, Z feed 12 | Klipper runtime/config ceilings differ: velocity 300, acceleration 5000, Z velocity 5, Z acceleration 100. Marlin-style declarations do not guarantee those limits are installed in Klipper. |

J01's start **exactly matches the latest previously captured S1 job start string**: absolute positioning / relative E, bed target and nozzle standby 150°C, wait for bed, home, **BED_MESH_CALIBRATE**, final nozzle target/wait, purge and E resets. Its end string also matches: conditional bed lowering, XY park, further conditional lowering, heaters/fan off and motor disable. No executable copy is published or run here.

Retain **fresh probing on every S1 print**, after homing and agreed thermal conditions. Do not substitute G29, omit probing, or load an old saved mesh over a fresh result. No START_PRINT/END_PRINT or G29 wrapper is loaded in the current S1 configuration. The project and historical dedicated-S1 profile labels are not text-identical, even though the compared start/end strings and selected values match; provenance of a particular generated job cannot be inferred solely from those matches.

J01 application metadata reports PrusaSlicer 2.9.5. Historical job headers include 2.9.6. That difference does not prove a downgrade, deletion or a second computer; it remains a source/version-history gap. No unsaved GUI changes or per-object/modifier settings were assessed.

## Nearby project evidence: avoid conflating profiles

| Recent-project alias | Availability / model | Relevant distinction |
|---|---|---|
| J01 | Accessible / ENDER5S1 / nozzle 0.5 | Explicit S1 project settings described above |
| J02 | Accessible / generic ENDER5 / nozzle 0.5, variant 0.4 | Uses G29 and a different end sequence; not a substitute S1 source. It includes bed-lowering template expressions while still in relative mode before returning to absolute mode, so travel semantics need review before any use. No such sequence was executed. |
| J03, J04, J06, J07 | Accessible / ENDER3S1 / nozzle 0.4 | Other-printer projects with differing thermal/retraction settings; not evidence for S1 hardware or calibration |
| J05, J08, J09 | Unavailable at saved references | Contents unknown; no replacement paths or results invented |

The presence of project-specific settings that differ from saved custom presets explains why auditing only those presets was incomplete. It does not establish which project is currently active or which combination is known-good. No whole-disk search, cloud retrieval, import or profile migration was performed.

## Meaning of the nozzle discrepancy

Klipper's nozzle declaration participates in extrusion safety/default calculations. The observed current cross-section limit is 0.64 mm². [Klipper's configuration reference](https://www.klipper3d.org/Config_Reference.html#extruder) describes this limit and related extruder settings. It is not a field to raise merely because a job fails or a slicer label differs. Extruder rotation distance, filament flow multiplier, physical nozzle identity and these safety limits are distinct checks.

Evidence favors **0.5 mm as the intended project/slicing setup**, not a proven measurement of the installed nozzle. Plausible outcomes after physical verification:

- If a 0.5 nozzle is confirmed, prepare a separate reviewed alignment proposal for firmware declaration and the chosen profile; review any resulting default/safety implications explicitly. Do not change rotation distance or relax cross-section limits automatically.
- If 0.4 is confirmed, review and re-slice a properly matched profile; do not edit old production G-code as a shortcut.
- If nozzle size remains unknown, do not label either side correct or begin nozzle-dependent calibration based on the software records alone.

## Recommended next review, with no actions applied

1. Owner confirms the fitted nozzle and whether J01 is an intended current S1 project. Keep original project and preset files private and unchanged.
2. In a separately approved workflow, recover the relevant project settings to an isolated candidate profile; do not import/save into or overwrite production presets automatically. Name the machine/nozzle deliberately and record the exact source/hash privately.
3. Prepare a proposed diff resolving flavour, actual nozzle declaration and useful bed limits while preserving S1-specific start/end and fresh probing every print. Keep generic Ender-5 and modified Ender-3 S1 behavior separate.
4. Review an offline slice of a synthetic model: bounds, temperature order, exactly one fresh mesh, E mode, purge/end geometry, PA and all acceleration/legacy commands. No automatic upload or test print.
5. Obtain separate authorization for any configuration save/restart and subsequent heating/movement/printing. Validate nozzle-dependent calibration and material flow/retraction; record a measured regression result before marking the candidate tested.

Rollback for any later approved experiment is reselecting the untouched original preset/project or restoring the specific authorized configuration backup in an idle window. This investigation requires **no rollback**, because source settings were not edited. Raw 3MF files embed models and potentially identifying metadata; do not publish them as profile evidence without a separate review.

## Review status

PR #6's current guide and description were separately checked and updated to remove personal camera narrative. It retains technical environment, backend diagnosis/fix, verification limitations and general diagnostics. Original Git commits have not been rewritten. This branch is based on that cleaned guide branch, so this new PR does not reintroduce the older narrative from sibling branches.

No claim here changes the acceptance status of the earlier audit or calibrations. Saved data and matching code are evidence for investigation, not physical test results.
