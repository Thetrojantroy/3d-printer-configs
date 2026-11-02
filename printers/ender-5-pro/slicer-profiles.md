# Ender 5 Pro slicer inventory

Status: **partial observed mapping, not tested**, 2 November 2026. The actual local PrusaSlicer 2.9.5 store was inspected read-only. See the [full audit](../../docs/slicer-audit-2026-11-02.md). No raw exports or private names published.

| Layer | Available values | Mapping limitation |
|---|---|---|
| Generic custom PR01 | ENDER5, 0.4 nozzle, Marlin; 0.8 mm retract at 30/30 mm/s | Not explicitly ENDER5PRO; owner must confirm actual use |
| Generic custom PR02 | ENDER5, 0.5 nozzle, Marlin; 2 mm retract at 30/30 mm/s | Also not explicitly Pro; variant metadata still says 0.4 |
| System Pro 0.4 / 0.5 | Installed vendor ENDER5PRO variants; 4 mm retract at 60/40 mm/s | Availability is not selection or calibration evidence; significantly customized Pro hardware is not stock by assumption |

All three custom printer files are standalone (empty inherits). No custom preset explicitly maps to ENDER5PRO. Current physical nozzle, real preset mapping, Marlin EEPROM limits and probe/leveling support remain unverified. Filament overrides can replace defaults, and generic model/vendor filters do not verify physical hardware.

PR01/PR02 use bed wait, G28, G29, final nozzle heating and purge; PR01 purges at negative X. Their end performs relative retract/wipe/Z raises, then XY homing and heat/fan shutdown. Confirm actual travel, end clearance and G29 support on this customized Marlin printer before adopting either. Do not replace these with a Klipper S1 macro or transplant another printer's end sequence.

Keep the existing controller pending identification/review. No profiles or machine settings changed. Follow the [test/rollback plan](../../docs/slicer-change-plan.md) and [hardware inventory](hardware.md) before any proposed correction or calibration.
