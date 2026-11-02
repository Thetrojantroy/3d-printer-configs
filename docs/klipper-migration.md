# Later Klipper migrations

Status: planning only. Ender 5 Pro and Ender 3 S1 currently use Marlin per the owner; no firmware conversion is authorized or provided here.

1. Complete the destination printer's hardware inventory. Read the **exact controller board model/revision and MCU marking physically**; record bootloader/flash method, communications, drivers, heater/sensor types, probe, endstops and wiring. Product name alone is insufficient.
2. Preserve a private Marlin baseline: firmware version/build source where available, settings report, slicer presets, current start/end behavior, physical nozzle and known-good print. Record a tested recovery path and available original firmware. Sanitize before any public summary.
3. Choose host and communications deliberately. Copy the S1 documentation structure and review process, not its pins, MCU build options, travel limits, thermistors, probe offsets, PID, mesh or tuned values.
4. Review a reference config for the exact board and firmware version. Mark draft files clearly and verify wiring against authoritative board documentation. Resolve every safety-critical TBD before flashing.
5. Obtain explicit approval for the proposed firmware build/flash and recovery procedure. Do not flash automatically or reuse another printer's binary. Later service changes/restarts also need their own planned window.
6. After an approved installation, verify sensor plausibility and endstop/probe states before approving movement or heating. Approve those tests separately with the owner present. Follow official [Klipper configuration checks](https://www.klipper3d.org/Config_checks.html).
7. Adapt the [calibration roadmap](calibration-roadmap.md) to the actual hardware. Preserve that printer's intended start/end and probing behavior; do not impose the S1's workflow without owner agreement.
8. Only label a configuration tested after recording actual checks and an accepted regression print. Keep recovery originals private and available throughout migration.
