# Hardware inventory

Status: owner report plus prior read-only observations; not a fresh physical inspection.

| Item | Value | Source / confidence |
|---|---|---|
| Printer | Creality Ender 5 S1 | Owner report |
| Controller / MCU | Exact physical revision and marking TBD | Prior configuration names a board/build target; not accepted as physical identification |
| Host | Raspberry Pi Zero 2 W, Raspberry Pi OS Bookworm | Prior local audit, 2026-11-02 |
| Printer stack | Klipper, Moonraker, Mainsail/Fluidd | Prior audit |
| Firmware | Klipper v0.13.0-269-g8db5d254 at audit | Historical software observation, not upgrade advice |
| Physical nozzle | Usually 0.5 mm | Owner statement in issue #1; current installation confirmation TBD |
| Declared nozzle | Firmware 0.4 mm; newest audited job 0.5 mm | Prior audit; mismatch unresolved here |
| Extruder / hotend / modifications | Exact physical details TBD | Configured extrusion distance is not hardware identification |
| Probe | CR Touch configured via bltouch | Prior configuration; physical offsets/repeatability need verification |
| Bed / surface / travel | Surface and physically verified usable travel TBD | Do not infer from model name alone |
| Heater/sensor/fan hardware | Physical part and wiring verification TBD | No changes proposed |
| Accelerometer | No configured sensor observed | Physical availability/mounting TBD |

Do not publish network identifiers or raw backups. No hardware inspection was performed for this repository change.
