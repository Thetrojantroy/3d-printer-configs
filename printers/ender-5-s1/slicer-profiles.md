# Slicer inventory

Status: historical inventory below, with a [read-only project/profile follow-up](../../docs/ender-5-s1-profile-nozzle-investigation.md). The follow-up found explicit S1 / 0.5 mm settings in a recent 3MF project, separate from the default saved custom-preset directory. No project or preset was imported, saved or changed. Raw exports and names remain private.

The recovered project retains fresh BED_MESH_CALIBRATE on every print and S1-specific start/end, but still uses Marlin flavour and a 224×224 bed outline versus current configured X/Y maxima 222/220. Its nozzle declaration is 0.5; current Klipper remains 0.4. Do not substitute a generic preset or apply a fix before verifying the physical nozzle and reviewing the proposed profile/config alignment.

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
