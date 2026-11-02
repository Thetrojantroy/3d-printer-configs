# 3D printer configurations and calibration

Central configuration and calibration resource for three printers, with balanced quality and speed as the goal. Use one `main` branch and printer directories; use short-lived branches and reviewed pull requests for changes.

| Printer | Current platform | Repository status |
|---|---|---|
| [Ender 5 S1](printers/ender-5-s1/README.md) | Klipper, Moonraker, Mainsail/Fluidd on Raspberry Pi Zero 2 W | Sanitized audit summary; no deployable configuration published |
| [Ender 5 Pro](printers/ender-5-pro/README.md) | Marlin; possible future Klipper migration | Inventory and migration templates; hardware details TBD |
| [Ender 3 S1](printers/ender-3-s1/README.md) | Marlin; possible future Klipper migration | Inventory and migration templates; hardware details TBD |

## Start here

1. Read the printer's hardware and slicer inventory. Confirm the physical nozzle; software cannot detect its diameter.
2. Follow the [calibration roadmap](docs/calibration-roadmap.md), recording evidence in the printer's calibration history.
3. Review [design decisions](docs/design-decisions.md), especially fresh probing on **every Ender 5 S1 print** and printer-specific start/end behavior.
4. Before publishing anything, follow the [public-data and secret review](docs/publication-safety.md).
5. Use the [migration checklist](docs/klipper-migration.md) for later Klipper migrations. Never copy another board's pin map blindly.

## Troubleshooting guides

- [Raspberry Pi Zero 2 W CSI camera: Crowsnest finds no usable devices or Mainsail has no feed](guides/raspberry-pi/raspberry-pi-zero-2-w-csi-camera-crowsnest.md) — confirmed OV5647/Bookworm case, actual repair, image checks and rollback.
- [Troubleshooting index](guides/README.md) — find guides by symptom and distinguish confirmed fixes from diagnostic possibilities.

## Layout and status

`printers/<printer>/` holds hardware details, slicer inventory, calibration history and setup notes. `shared/` holds reusable documentation templates, not universal machine settings. `docs/` holds project-wide decisions and procedures.

- **Observed:** a dated reading or owner report, with its source stated. Not necessarily calibrated or physically verified.
- **Tested:** a recorded hardware-specific test with conditions, results and acceptance. Valid only for that recorded setup.
- **Draft/proposed:** not for execution or deployment until reviewed and approved.
- **TBD:** unknown; do not substitute a guess.

This initial contribution is documentation only. It includes no firmware, ready-to-run printer configuration, executable calibration G-code or automatic deployment. Nothing here authorizes flashing, heating, movement, restarts or changes to a working printer.

## Contributing

Create a short-lived branch, update the relevant printer records, inspect the full staged diff, run a secret scan, and open a pull request. Do not merge without owner review. Include what was tested, what remains proposed, sanitized evidence, and a rollback plan for any future operational change. Keep original private backups outside this public repository.
