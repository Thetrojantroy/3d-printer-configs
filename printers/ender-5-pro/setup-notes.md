# Setup and migration notes: Ender 5 Pro

Status: proposed planning only. Current firmware: Marlin (owner report); exact version/build, controller/MCU, host, modifications and recovery method: TBD.

Before a Klipper proposal, physically identify the controller board revision and MCU, capture a private settings/firmware baseline, inventory current slicer presets and document existing start/end and leveling behavior. Preserve the working setup and prove a recovery route.

Follow the [migration checklist](../../docs/klipper-migration.md). Reuse documentation fields and review practices, not another printer's pins or firmware binary. Obtain explicit approval before flashing, heating or movement; plan service changes separately. No migration operation is authorized by this document.

Open questions: board/MCU, sensors/heaters, probe/endstops, nozzle/extruder/hotend, travel, wiring, firmware build/bootloader, communications, calibration history and current printer-specific start/end behavior are all TBD.
