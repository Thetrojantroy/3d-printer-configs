# Troubleshooting guides

Reusable, symptom-focused guides live here; machine inventories and calibration history remain in `printers/`. Do not duplicate a guide for each printer.

| Symptom / search terms | Guide | Evidence and limits |
|---|---|---|
| Raspberry Pi Zero 2 W CSI camera, Bookworm, Crowsnest "No usable Devices Found", Mainsail no video, Unicam pipeline -22 | [Pi Zero 2 W CSI camera and Crowsnest](raspberry-pi/raspberry-pi-zero-2-w-csi-camera-crowsnest.md) | Recorded OV5647 repair; includes transient first-frame corruption and rollback. Not a universal camera fix. |

`raspberry-pi/` contains host/camera problems. [Klipper guide scope](klipper/README.md) describes the space for firmware-specific troubleshooting. The [calibration roadmap](../docs/calibration-roadmap.md) is planning, not a troubleshooting result.

Every guide should identify its verified environment, symptoms, evidence, actual fix, verification and remaining limits. Label other possible causes as diagnostic branches, not confirmed findings. See [publication safety](../docs/publication-safety.md) before adding examples or evidence.
