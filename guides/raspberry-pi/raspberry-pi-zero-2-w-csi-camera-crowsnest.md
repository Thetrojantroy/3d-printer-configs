# Raspberry Pi Zero 2 W CSI camera missing in Crowsnest / Mainsail on Bookworm

## Scope and evidence

Status: **recorded repair**, performed on 2 November 2026; this documentation PR does not operate a printer. Source: local Pi-Camera-Fix diagnosis notes, before/after Crowsnest copies, repair command output, camera/direct-capture checks and final service checks. These were inspected before writing the guide. Raw logs, camera pictures, personal paths, network details and working backups remain private.

Verified environment:

| Component | Recorded value |
|---|---|
| Host | Raspberry Pi Zero 2 W, Raspberry Pi OS Bookworm, 32-bit armv7l |
| Kernel | 6.12.34+rpt-rpi-v7 |
| Camera after connection | OV5647, reported at `/base/soc/i2c0mux/i2c@1/ov5647@36` |
| Utilities | rpicam-apps 1.9.0; libcamera runtime reported v0.5.2+99-bfd68f78 |
| Crowsnest | v4.1.16-1-g059fa87; installed ustreamer v6.36 and camera-streamer revision 400e036 |
| Printer stack | Klipper v0.13.0-269-g8db5d254, Moonraker, Mainsail and Fluidd |

This is not a recommendation to downgrade or install these versions. Other sensors, Pi models, distributions and Crowsnest builds need their own diagnosis. The camera Device Tree path below is a hardware path, not a private filesystem location.

## Symptoms and confirmed causes

Two different conditions occurred:

1. **Initially no physical camera was connected.** `rpicam-hello --list-cameras` reported no cameras, CSI nodes were disabled, and Crowsnest stopped with "No usable Devices Found". Mainsail's snapshot proxy returned HTTP 502. Matching sensor modules and camera software were already installed; installing packages would not provide a missing camera.
2. After a camera was connected with power removed and the Pi powered up, OV5647 auto-detection worked. But the default camera section used `mode: ustreamer` and `device: /dev/video0`. The kernel repeatedly reported `Wrong width or height 640x480 (remote pad set to 1920x1080)` and `Failed to start media pipeline: -22`. The raw CSI V4L2 endpoint was being used without the appropriate libcamera pipeline. Switching that camera to the installed camera-streamer backend with its detected libcamera path resolved ongoing capture.

An active service and even HTTP 200 were insufficient proof: an endpoint could return a bad startup frame. The actual image had to be inspected.

## Prerequisites and read-only checks

Use an authenticated local terminal/SSH session and access to your Crowsnest config. Do not publish credentials or expose printer/SSH ports to the internet. Verify the printer is idle before any service or hardware operation; do not interrupt an active print. Obtain owner approval before shutdown/reboot or any printer service changes.

Recorded diagnostic commands (run on the Pi):

```sh
uname -a
cat /etc/os-release
rpicam-hello --list-cameras
systemctl is-active klipper moonraker nginx crowsnest
systemctl status crowsnest --no-pager -l
journalctl -u crowsnest -b --no-pager -n 100
v4l2-ctl --list-devices
sudo -n dmesg | grep -Ei 'camera|imx|ov5647|unicam|csi|under.volt'
vcgencmd get_throttled
```

Inspect your actual Crowsnest config locally as well. Paths vary: this installation used `~/printer_data/config/crowsnest.conf`. If `sudo -n` refuses, it has not obtained permission; do not paste a password into a log or chat. `vcgencmd get_camera` was also checked but was not used alone to decide modern libcamera detection.

### If no camera is listed

First confirm a compatible camera and the correct Pi Zero CSI ribbon are physically present. In the recorded case, the owner approved a clean shutdown, power was removed, the camera connected, and power restored. **Do not attach or reseat the ribbon while powered.** Do not pull power before a clean shutdown.

The boot configuration already contained `camera_auto_detect=1`; it was not edited. The connected camera was then detected without adding a manual overlay. Wrong ribbon orientation, loose/damaged cable, another sensor needing an overlay, unavailable modules or power problems are *other diagnostic possibilities*, not causes established in this case. Investigate them only if your evidence points there. Do not add a guessed sensor overlay or enable a legacy stack as a blanket remedy.

## Actual configuration repair

Only follow this branch if the camera is listed and your evidence supports the backend mismatch. The installation already had camera-streamer; no package installation, OS upgrade, firmware change or Klipper configuration edit was needed.

The recorded backup operation was:

```sh
backup="$HOME/printer_data/config/crowsnest.conf.before-csi-repair-$(date +%Y%m%d-%H%M%S)"
cp -p "$HOME/printer_data/config/crowsnest.conf" "$backup"
printf 'BACKUP=%s\n' "$backup"
```

Retain the printed backup path privately. These commands assume the recorded config layout; locate yours first. The repair replaced **only these two keys in the existing `[cam 1]` section**:

```diff
 [cam 1]
-mode: ustreamer
-device: /dev/video0
+mode: camera-streamer
+device: /base/soc/i2c0mux/i2c@1/ov5647@36
```

The existing `port: 8080`, `resolution: 640x480` and `max_fps: 15` were retained. Use **your own detected path** from `rpicam-hello --list-cameras`, not an assumed OV5647 path. Edit the existing section; do not append a second `[cam 1]` or overwrite unrelated camera settings. In the inspected installed Crowsnest code, this `/base/soc` camera path selected the libcamera backend. Confirm support in your own installed build if behavior differs.

Then the actual service command was:

```sh
sudo -n systemctl restart crowsnest
```

This restarts camera streaming only and interrupts its viewers. Klipper and Moonraker were **not** restarted. No software-repair reboot was required; the earlier power cycle was for safely connecting hardware. No post-repair reboot test was performed.

## Verification through the existing Mainsail route

The existing webcam entry used `/webcam/?action=stream` and `/webcam/?action=snapshot`; it did not need changing. On the Pi the recorded checks included:

```sh
curl -fsS --max-time 15 -o /tmp/camera-check.jpg \
  -w 'snapshot_http=%{http_code} bytes=%{size_download}\n' \
  'http://127.0.0.1/webcam/?action=snapshot'
file /tmp/camera-check.jpg
curl -s --max-time 4 -o /tmp/camera-check.mjpg \
  -w 'stream_http=%{http_code} bytes=%{size_download}\n' \
  'http://127.0.0.1/webcam/?action=stream'
```

The original temporary output names were normalized here; URLs, test parameters and config changes retain the recorded behavior. The snapshot was a 640×480 JPEG. The bounded stream check returned HTTP 200 and approximately 2.85 MB in four seconds. A timeout at the end of a deliberately bounded continuous-stream check is expected; **a timeout alone is not success**—inspect status, received bytes and decoded frames.

View actual consecutive snapshots and the moving feed in Mainsail, not just `file` output. From your client use the printer's existing local Mainsail address and the paths above. Do not publish that address. The recorded checks verified the configured snapshot/stream routes and clear images; browser UI playback was not independently visually inspected in that session. The owner subsequently confirmed the camera was fixed.

The first snapshot after startup was sometimes corrupt (purple/black artifact). Three subsequent snapshots were clear. That initial-frame artifact was **not eliminated**, so the result is working ongoing output with a known startup limitation, not a claim of flawless first-frame behavior.

An independent isolation test was also performed: stop only Crowsnest, run `rpicam-still -n -t 3000 --width 640 --height 480 -o /tmp/camera-direct.jpg`, then start Crowsnest again. Its photo was clear. This is an optional diagnostic, interrupts camera viewers and must ensure Crowsnest is started again even if capture fails. Do not run two competing capture processes against the camera.

Final checks showed Klipper, Moonraker, nginx and Crowsnest active, the printer ready, Mainsail/Fluidd returning HTTP 200 and `throttled=0x0`. A service status alone does not validate an entire printer operation or justify starting a print.

## Rollback and remaining risks

If this camera-specific change fails, restore your exact saved backup over the same Crowsnest config and restart only Crowsnest. For the backup variable above, **in the same shell**, the inverse operation is:

```sh
test -n "$backup" && test -f "$backup" && \
  cp -p "$backup" "$HOME/printer_data/config/crowsnest.conf" && \
  sudo -n systemctl restart crowsnest
```

In a new shell, set `backup` to the actual previously saved path first; never choose a guessed timestamp. Restoring the baseline in this case restores a backend that did not work with the CSI sensor, so rollback preserves the old configuration rather than guaranteeing video. Keep a copy of any later edits before restoring an older backup.

This guide does not alter printer pins, safety limits, firmware, boot overlays or slicer profiles. It does not diagnose every cause of no video, approve a reboot or shutdown on another system, or justify a broad reinstall. Start with evidence and keep unrelated working printer services untouched.
