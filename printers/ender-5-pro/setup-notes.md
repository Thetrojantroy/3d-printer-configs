# Setup and migration notes: Ender 5 Pro

Status: proposed planning only. Current firmware: Marlin (owner report); exact version/build, controller/MCU, host, modifications and recovery method: TBD.

Before a Klipper proposal, physically identify the controller board revision and MCU, capture a private settings/firmware baseline, inventory current slicer presets and document existing start/end and leveling behavior. Preserve the working setup and prove a recovery route.

Follow the [migration checklist](../../docs/klipper-migration.md). Reuse documentation fields and review practices, not another printer's pins or firmware binary. Obtain explicit approval before flashing, heating or movement; plan service changes separately. No migration operation is authorized by this document.

## Customized hardware gate

Per the owner's 2 November 2026 report in [issue #4](https://github.com/Thetrojantroy/3d-printer-configs/issues/4), this printer is significantly customized and needs a full modification inventory. Retain the existing controller board unless its identification and review justify another choice. Board replacement is not a migration prerequisite.

Do not assume Ender 5 S1/Ender 3 S1 firmware, pin mapping, probe geometry, sensors or motion settings apply. Physically identify the exact board revision/MCU and document wiring and every installed modification before any build or flash proposal.

Open questions: modification list, exact board/MCU, sensors/heaters, probe/endstops, nozzle/extruder/hotend, travel, wiring, firmware build/bootloader, communications, calibration history and current printer-specific start/end behavior remain TBD.
