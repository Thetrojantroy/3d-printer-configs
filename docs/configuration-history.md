# Configuration and profile history

Dates describe source evidence, not proof of successful printing. Original backups, filenames/alias mapping and hashes stay in private local storage outside public Git.

| Evidence date / source | Observation | Status / interpretation |
|---|---|---|
| 2023-12-11, custom printer file headers | All three current custom printer files report generation by PrusaSlicer 2.7.0+win64 | Header metadata only; does not prove when hardware last changed |
| 2024-11-08 and 2025-01-10 slicer snapshots | Each contains 37 comparable custom printer/print/filament files; all 37 match current custom files by hash | Historical equality of saved files, not test certification; vendor and application settings can differ |
| Earlier 2026 stored S1 job audit | 31 jobs; newest metadata reports 2.9.6, a dedicated S1-labelled profile, fresh probing and a 0.5 nozzle declaration | Historical job evidence; not the same as current saved local profiles |
| 2026-11-02, owner clarification | S1 usually has 0.5 nozzle; original hotend/printhead; gear replacement planned but not installed. Other hardware modifications documented separately | Owner report from issues #1/#4, not physical measurement |
| 2026-11-02, local saved-profile audit | Running/installed and preference-file version 2.9.5; 3 custom printer, 28 print, 6 filament files. Saved selection PR03/PT16/FI04 | Read-only observation; unsaved/project overrides not inspected |
| 2026-11-02, current S1 config recheck | Main and Mainsail include hashes match the prior private baseline; PA zero, no shaper, nozzle declaration 0.4 | No printer settings changed; physical nozzle still needs confirmation |

Version metadata alone cannot establish why 2.9.6 jobs and a 2.9.5 local installation coexist. Do not invent a downgrade, missing deletion or another computer. The original source of the dedicated S1 preset remains a gap.

## Known-good versus experimental

- **Working installation:** the owner reports the Klipper S1 and camera working; this does not certify every slicer preset.
- **Observed profiles:** all PR/PT/FI aliases in the [audit](slicer-audit-2026-11-02.md). None is promoted to known-good by this contribution.
- **Experimental/proposed:** all future nozzle alignment, filter fixes, thermal/start/end changes, calibrated flow/PA/retraction and macro refactors. No experimental preset was created or enabled during this audit.
- **Tested/accepted:** no new physical calibration result. Add evidence and owner acceptance before using this status.

Future history entries should reference sanitized baseline identifiers, actual changes/approval, test file, conditions, measurements, acceptance and rollback. Retain a separately named experimental copy rather than overwriting the production preset. A successful slice or a matching hash is not a successful physical print.
