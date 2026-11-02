# Slicer inventory

Status: historical job evidence below, supplemented by the [actual local read-only profile audit of 2 November 2026](../../docs/slicer-audit-2026-11-02.md). Profiles were read for issue #5; none were changed. Raw names/exports remain private.

## Current local-store mapping gap

The local 2.9.5 store has no custom printer preset explicitly declaring ENDER5S1: PR01/PR02 are generic ENDER5 Marlin variants (0.4/0.5), and their mapping to physical machines is unconfirmed. The installed system Ender-5 S1 0.4/0.5 variants are also Marlin-oriented and omit fresh probing. They must not silently replace the historical Klipper S1 profile. Its actual saved source/export remains missing. The prior 2.9.6 job metadata is historical, not evidence that those settings exist in this store today.

Current S1 configuration was re-read: main/include hashes match the earlier baseline, PA remains zero, shaping absent and nozzle declaration 0.4. No start/end macro or G29 wrapper is loaded. The intentional fresh mesh on **every print** must be retained when the actual S1 preset is identified; do not infer G29 compatibility from a generic preset.

| Category | Evidence | Next verification |
|---|---|---|
| Printer / nozzle presets | Newest audited job declares 0.5 mm and Marlin flavour | Confirm current physical nozzle and selected preset; review Klipper flavour in a separate change |
| Filament presets | Newest audited job has PET-type metadata, nozzle 235°C, bed 75°C, flow multiplier 1.0 | Material/spool and current profile identity TBD; temperatures are observations, not recommendations |
| Print presets | Newest audited job uses 0.4 mm layers and 0.8 mm widths | Preserve intentional presets; select controlled settings for tests |
| Retraction | Newest audited job uses 1 mm at 30 mm/s, extra restart +0.2 mm | Physically validate after PA; do not silently remove |
| Flow cap | Newest audited job declares volumetric caps zero | Characterize actual flow and propose a conservative cap later |

## Observed start/end behavior

Prior local audit: all 31 stored jobs call fresh BED_MESH_CALIBRATE. The newest starts by waiting for the bed, holding the nozzle at a lower standby temperature, homing, probing, heating the nozzle to final temperature and purging. Preserve **fresh mesh on every print**, not a saved-only workflow. Temperature stabilization is not a defined soak interval.

The newest embedded end behavior lowers the bed where travel permits, parks XY, lowers further conditionally, turns heaters/fan off and disables motors. It is recorded as historical behavior, not executable instructions or proof of physical safety. Preserve machine-specific intent and validate coordinates before later refactoring.

The scan found M204 S1500 and S500 in all 31 jobs and S250 in 30. These change acceleration and can undermine calibration tests. M201/M205 were emitted in all jobs; M203 was seen in newest start excerpts but not counted across all files. The audit found no corresponding compatibility macros. Do not interpret their Marlin-style machine limits as active Klipper limits.

No active PA or shaping overrides matched the recorded scan. This does not cover unsaved profiles, deleted jobs or future files. Keep ownership of each setting explicit: firmware mechanical baseline; validated material-specific values in named profiles or deliberate macros; no hidden competing overrides.

## Export checklist

Record sanitized preset names, slicer version/date, nozzle confirmation, start/end review, supported commands, test status and hash for each exported profile. Check connection settings and personal metadata before publication. Apply the [proposed consistency check](../../docs/design-decisions.md), not automatic changes to safety limits.
