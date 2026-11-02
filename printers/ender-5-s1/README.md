# Ender 5 S1

Current platform: Klipper with Moonraker and Mainsail/Fluidd on Raspberry Pi Zero 2 W. Owner reports a usual physical nozzle of **0.5 mm**; confirm what is fitted before each nozzle-specific test. Existing PrusaSlicer printer/nozzle/filament presets should be inventoried, not replaced blindly.

Status: documentation and observed baseline only; no deployable configuration is published. The prior local audit observed PA zero and no input shaping, pending verification. Fresh bed probing on **every print** is intentional and must remain in this printer's workflow.

- [Hardware inventory](hardware.md)
- [Slicer inventory and start/end behavior](slicer-profiles.md)
- [Calibration history and audit provenance](calibration-history.md)
- [Setup notes](setup-notes.md)
- [Staged roadmap](../../docs/calibration-roadmap.md)

Keep the working installation untouched until a separately reviewed and approved calibration stage. Read the [nozzle warning design](../../docs/design-decisions.md) before changing declarations or safety parameters.
