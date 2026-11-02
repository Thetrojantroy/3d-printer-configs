# Setup and future change notes

Status: working installation exists; repository contribution is documentation only.

The prior audit found a main printer configuration including the standard Mainsail macro file. Existing private backups and raw records remain outside this repository. No deployable sanitized configuration has been reviewed/published yet. Exact board/MCU identification, recovery method and hardware modifications remain TBD pending confirmation.

Preserve Klipper/Moonraker/Mainsail/Fluidd and the working camera installation. Do not reuse camera-access credentials or publish network identifiers. Do not restart services or operate the printer from a documentation task.

The recorded camera repair is documented once in the reusable [Pi Zero 2 W CSI/Crowsnest guide](../../guides/raspberry-pi/raspberry-pi-zero-2-w-csi-camera-crowsnest.md), including its first-frame limitation. Do not duplicate raw diagnostics here.

Before a future configuration PR: identify affected files and includes; preserve a private rollback copy; establish physical nozzle/profile alignment; review the full diff; retain fresh probing on every print and printer-specific start/end behavior; obtain approval for any flashing, heating, movement or required restart. Verify the accepted result with recorded evidence before marking it tested.

The shared [roadmap](../../docs/calibration-roadmap.md) governs future calibration. Migration practices for the other printers are in the [migration checklist](../../docs/klipper-migration.md); this S1's pins and offsets are not a migration template.
