# Setup and migration notes: Ender 3 S1

Status: proposed planning only. Current firmware: Marlin (owner report); exact version/build, controller/MCU, host, modifications and recovery method: TBD.

Before a Klipper proposal, physically identify the controller board revision and MCU, capture a private settings/firmware baseline, inventory current slicer presets and document existing start/end and leveling behavior. Preserve the working setup and prove a recovery route.

Follow the [migration checklist](../../docs/klipper-migration.md). Reuse documentation fields and review practices, not another printer's pins or firmware binary. Obtain explicit approval before flashing, heating or movement; plan service changes separately. No migration operation is authorized by this document.

## Installed modifications and geometry gate

The owner reports in [issue #4](https://github.com/Thetrojantroy/3d-printer-configs/issues/4), dated 2 November 2026, that a Micro Swiss all-metal hotend and custom printed printhead/mount are fitted; the original extruder remains. These are owner reports, not physical measurements.

The owner's approximate-stock nozzle/CR Touch position is **unverified**. Measure X/Y nozzle-to-probe offsets, probe deployment/clearance and usable probe/nozzle travel before a Klipper migration or any change that depends on those positions. Do not populate stock numeric offsets from another S1 configuration. Review the custom mount's rigidity and changed heater/sensor/fan wiring before approved tests. No offsets are changed here.

Remaining questions: exact board/MCU, hotend part/version, sensors/heaters, measured probe/endstop geometry, nozzle diameter, travel, wiring, firmware build/bootloader, communications, calibration history and current printer-specific start/end behavior.
