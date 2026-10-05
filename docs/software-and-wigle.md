# Scout Lite software and WiGLE guide

[← Scout Lite overview](../README.md) · [Troubleshooting](troubleshooting.md) · [Web Flasher](https://flash.pingequa.com/devices/scout-lite)

Scout Lite ships with **ESP32 Marauder**. Marauder and SigRoam are alternative ESP32-C5 images; the SigRoam **FAP** is a separate app on the Flipper Zero. Pick the combination for your job before installing anything.

## Which software runs where?

| Scout Lite ESP32-C5 | Flipper Zero | What to expect |
| --- | --- | --- |
| **Factory Marauder** | **Marauder companion app** | Recommended starting point for the broader Wi-Fi research toolkit and Marauder Wardrive. |
| **Factory Marauder** | **SigRoam Wardriving FAP** | Dedicated Flipper field display over Marauder's serial protocol. Marauder still creates logs and handles uploads. |
| **Optional SigRoam scanner** | **Matching SigRoam FAP** | Passive survey capture, GNSS-gated sealed logs, session health views, and configured WiGLE upload attempt after STOP. |

The Flipper controls and displays. Scout Lite scans Wi-Fi, reads onboard GNSS, and writes to **its own microSD**. The Flipper's SD card stores FAP apps and Flipper settings, not the scanner's survey CSV.

## Factory Marauder and its companion app

1. Attach the included dual-band antenna, insert a FAT32 microSD in Scout Lite, and mount the board on the Flipper GPIO header.
2. Open a compatible Marauder companion app. On Momentum, look under **Apps → GPIO → [ESP32] WiFi Marauder**. The board is already flashed.
3. Confirm the app sees the board with a normal scan. Select **Wardrive** for location-tagged observations and wait for a valid GNSS fix outdoors.
4. Stop and read the WigleWifi CSV on the Scout Lite card. See the upstream [Marauder Wardrive guide](https://github.com/justcallmekoko/ESP32Marauder/wiki/Wardrive) for its GPS-gated log behavior.

For on-board WiGLE sending, Marauder has a **separate, manually triggered** [Direct Upload workflow](https://github.com/justcallmekoko/ESP32Marauder/wiki/wardriving-direct-upload). Put `wigle_api_name.txt` and `wigle_api_token.txt` on the Scout Lite card, join Wi-Fi using Marauder's flow, and trigger upload from its menu. Marauder may save Wi-Fi profiles; follow the current upstream guide for your installed image. You can also remove the card after powering down and upload its compatible CSV through [WiGLE](https://wigle.net/).

## SigRoam FAP with factory Marauder

The [SigRoam Wardriving FAP](https://github.com/pingequalab/sigroam-wardriving) speaks Marauder's serial protocol, so the factory image can stay in place. This is the least disruptive way to try SigRoam's Flipper dashboard.

1. Choose the FAP asset for your Flipper firmware from [SigRoam Releases](https://github.com/pingequalab/sigroam-wardriving/releases/latest). Copy it to `SD Card/apps/GPIO/` on the **Flipper**.
2. Set **Settings → System → Log Device → Off** so the Flipper system logger does not occupy the serial pins. Open **Apps → GPIO → SigRoam Wardriving**.
3. Run **Probe firmware** if needed. In Dashboard, press **OK** to start and again to stop; **Back** only leaves the dashboard.
4. Read logs and upload them using **Marauder's** workflow. The SigRoam FAP alone does not create sealed SigRoam sessions or perform SigRoam's after-STOP upload.

Check [SigRoam's compatibility and limits](https://github.com/pingequalab/sigroam-wardriving#compatibility-and-limits) for the current FAP assets and supported Flipper firmware.

## Complete SigRoam workflow

This option changes the firmware on Scout Lite and pairs it with the matching FAP. Its survey capture is passive; it has no deauthentication, handshake capture, or attack features. Uploading a finished survey later uses ordinary Wi-Fi/HTTPS traffic.

1. Disconnect Scout Lite from the Flipper. Open the [Scout Lite Web Flasher](https://flash.pingequa.com/devices/scout-lite) in a browser it currently supports.
2. Enter download mode as instructed there: hold **BOOT**, plug in a USB-C **data** cable while holding it, release BOOT, then choose **SigRoam** and connect. Let the picker write the correct image layout.
3. Install the matching [SigRoam FAP](https://github.com/pingequalab/sigroam-wardriving/releases/latest) on the Flipper. The FAP and scanner image are different downloads; do not flash a FAP to the ESP32-C5.
4. Insert a FAT32 microSD in **Scout Lite**, set **Log Device → Off** on the Flipper, then open **SigRoam Wardriving → Dashboard**.
5. Press **OK** to start, watch GNSS and session status, and press **OK** to stop. **Back** only changes the Flipper view.

The [scanner firmware project](https://github.com/pingequalab/sigroam-firmware) documents release-specific scanner behavior. Scout Lite is the supported reference board. The Web Flasher also offers **Marauder** if you want to switch back; use its picker instead of mixing raw binary offsets.

## Where logs and upload credentials live

**Both firmwares write their wardrive output to the microSD inserted in Scout Lite.** The Flipper's SD card is a separate device. To enable SigRoam scanner upload, create these plain-text files at the **root of the Scout Lite card**, each containing only the named value:

| File | Content |
| --- | --- |
| `wigle_api_name.txt` | WiGLE API Name from your [WiGLE account](https://wigle.net/account) |
| `wigle_api_token.txt` | WiGLE API Token |
| `home_ssid.txt` | Wi-Fi network name used for upload |
| `home_psk.txt` | Wi-Fi password; empty file for an open network |

Restart Scout Lite to import them. The SigRoam scanner consumes the source files; check that they disappear and that the FAP **Upload** page reports **Key** and **Home** configured. Keep these files and their contents private.

After a **successful STOP**, SigRoam seals and verifies the CSV, then **attempts** to upload pending sealed files if the configured network is reachable. Keep Scout Lite powered and the microSD inserted. Failed uploads remain on the card for a manual retry from the FAP **Upload** page. **Uploaded** with a WiGLE `transId` means WiGLE accepted the file; later processing and scoring are separate. The scanner does not interrupt a running survey or automatically start another attempt merely because Wi-Fi later returns.

**Marauder direct upload and SigRoam after-STOP upload are different paths.** Installing the SigRoam FAP while Marauder is still on the board does not change Marauder's CSV or upload behavior.

## GPS and band limits

- Scout Lite's onboard L86-M33 GNSS sends data to the ESP32-C5, which Marauder or SigRoam uses for wardriving. Start outdoors and wait for a valid fix. Marauder's [Wardrive guide](https://github.com/justcallmekoko/ESP32Marauder/wiki/Wardrive) says log rows are written only while GPS has a valid fix.
- The separate native Flipper GPS app needs the board's NMEA feed mapped to **GPIO 15/16**. On Momentum, choose **MNTM → Protocols → GPIO Pins → NMEA GPS UART → Extra 15,16**. This mapping is not required for board-side wardriving.
- The ESP32-C5 uses one Wi-Fi radio for 2.4 GHz and 5 GHz scans. It does not survey both bands at once; 5 GHz is scan/enumeration only on this product, not 5 GHz handshake capture.

If a step fails, use the [troubleshooting checklist](troubleshooting.md). For current versions and Flipper API compatibility, follow the [FAP release](https://github.com/pingequalab/sigroam-wardriving/releases/latest), [scanner release](https://github.com/pingequalab/sigroam-firmware/releases), and [Web Flasher](https://flash.pingequa.com/devices/scout-lite) rather than a version copied into an older guide.
