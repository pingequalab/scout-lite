# Scout Lite for Flipper Zero

**Compact ESP32-C5 dual-band Wi-Fi wardriving board for the Flipper Zero.** Scan 2.4 and 5 GHz, tag observations with onboard GPS, and save WiGLE-ready CSV to the Scout Lite microSD. Scout Lite ships with ESP32 Marauder; the optional SigRoam scanner firmware adds an after-stop WiGLE upload workflow.

> For network research, security auditing, and education only. Only scan or test networks you own or are explicitly authorized to assess.

---

## What it is

Scout Lite is a streamlined Scout variant focused on **Wi-Fi · GPS · logging**. It runs pre-flashed ESP32 Marauder and works the moment you mount it on the Flipper's GPIO header — no assembly, no soldering. CC1101 and nRF24 are removed; a microSD slot is added for standalone WigleWifi logging.

## Hardware at a glance

| | |
|---|---|
| **Wi-Fi SoC** | ESP32-C5-WROOM-1U — 2.4 + 5 GHz Wi-Fi 6 (5 GHz is scan / enumeration only) |
| **GPS** | Quectel L86-M33 (MediaTek MT3333) — GPS / GLONASS / Galileo, integrated patch antenna, −165 dBm tracking |
| **Storage** | microSD (FAT32) — WigleWifi CSV. *Card not included.* |
| **Antenna** | SMA connector + dual-band 2.4 / 5 GHz antenna included |
| **Interface** | Flipper Zero GPIO header · USB-C (power + flashing) |
| **Firmware** | ESP32 Marauder — ships pre-flashed |
| **Flipper app (optional)** | [SigRoam](https://github.com/pingequalab/sigroam-wardriving) — dedicated receive-only wardriving dashboard |
| **Dedicated scanner firmware** | [SigRoam 0.6](https://github.com/pingequalab/sigroam-firmware/releases/tag/v0.6), optional in the [Scout Lite Web Flasher](https://flash.pingequa.com/devices/scout-lite); factory units ship with Marauder |

## Getting started

1. **Mount** Scout Lite on the Flipper Zero's GPIO header. It ships pre-flashed — nothing to install on the board.
2. **Install [Momentum](https://momentum-fw.dev)** firmware on the Flipper. Momentum bundles the Marauder companion app and can remap the GPS UART to GPIO 15/16 (stock firmware cannot).
3. **Insert a microSD card** (FAT32, not included). Logs are written here.
4. On the Flipper, open **Apps → GPIO → [ESP32] WiFi Marauder**.
5. Select **Wardrive**. Access points are logged with time, signal, channel, and GPS coordinates as WigleWifi CSV.

## Choose a Flipper wardriving workflow

The [SigRoam Flipper app](https://github.com/pingequalab/sigroam-wardriving) is a `.fap`; the [SigRoam scanner firmware](https://github.com/pingequalab/sigroam-firmware) is an ESP32-C5 image. They are different downloads. The Flipper controls and displays the survey; Scout Lite scans, reads GPS and writes its own microSD.

| Scout Lite firmware | Flipper app | WiGLE upload |
|---|---|---|
| Factory ESP32 Marauder | Marauder companion or compatible SigRoam FAP | Marauder's direct-upload workflow, or copy its CSV from Scout Lite microSD. SigRoam's after-stop auto-upload does not apply. |
| Optional SigRoam scanner firmware | SigRoam FAP | Configure WiGLE and upload Wi-Fi credentials on Scout Lite microSD once; after a successful STOP, the scanner attempts to upload sealed files when that Wi-Fi is reachable. Retry from the FAP Upload page if needed. |

To use the complete SigRoam workflow, select **SigRoam** in the [Scout Lite Web Flasher](https://flash.pingequa.com/devices/scout-lite) and install the matching FAP from [SigRoam Releases](https://github.com/pingequalab/sigroam-wardriving/releases/latest) to `SD Card/apps/GPIO/` on the **Flipper**. Release 0.6 provides an Official/Momentum-compatible `.fap` and a separate Unleashed build; choose the asset for your Flipper firmware. Set **Settings → System → Log Device → Off** to free serial pins 13/14, then open **Apps → GPIO → SigRoam Wardriving → Dashboard**. The dedicated scanner firmware is supported on Scout Lite; do not flash the Flipper `.fap` to the ESP32.

SigRoam survey capture is receive-only: no deauth, handshake capture or attack features. Uploading later uses a normal Wi-Fi connection.

## GPS wiring & the Flipper GPS app

The onboard GPS is hard-wired to the ESP32-C5 (so Marauder logs location automatically), and its NMEA output is also routed to the Flipper's GPIO 15/16.

To view live position in the Flipper's native GPS app, enable the pin mapping:

> **Momentum:** MNTM → Protocols → GPIO Pins → NMEA GPS UART → **Extra 15,16**

Wardriving through Marauder works whether or not this is enabled. Stock Flipper firmware pins the GPS app to GPIO 13/14 — which the ESP32 already uses — so the native GPS app needs Momentum or Xtreme.

## Uploading logs to WiGLE

### SigRoam scanner: configure on the Scout Lite microSD

Get the API Name and API Token from your [WiGLE account](https://wigle.net/account). Put four plain-text files in the **root of the Scout Lite microSD**, each containing only its value:

| File | Value |
|---|---|
| `wigle_api_name.txt` | WiGLE API Name |
| `wigle_api_token.txt` | WiGLE API Token |
| `home_ssid.txt` | Wi-Fi name used for uploads |
| `home_psk.txt` | That Wi-Fi password; empty file for an open network |

Restart Scout Lite to import the files. The scanner overwrites and removes the source files after import; check that they disappeared and that the FAP **Upload** page shows `Key` and `Home` configured. Do not post the files or their contents publicly. Start a survey in **Dash**, then press OK to stop. SigRoam seals and verifies the CSV and automatically **attempts** to upload pending sealed files if the configured Wi-Fi is reachable. Keep the board powered and its microSD inserted while sending. `Uploaded` with a WiGLE `transId` means WiGLE accepted the file; later processing and scoring are separate. If offline, use **Upload** on the Flipper to retry when back in range. SigRoam does not interrupt an active survey to upload or automatically start another attempt just because the Wi-Fi appears later.

### Factory Marauder: join Wi-Fi and trigger direct upload

Marauder also supports direct WiGLE upload. Put `wigle_api_name.txt` and `wigle_api_token.txt` on the Scout Lite microSD root, then use Marauder's **Join WiFi** flow after an AP scan and trigger the upload from its menu. The official CLI documents `join -a <ap_index> -p <password>`; on a Flipper-controlled board, selecting the network and entering its password through the app is an extra setup step. Marauder can save Wi-Fi profiles, so this is not necessarily repeated on every upload. The [Marauder Direct Upload guide](https://github.com/justcallmekoko/ESP32Marauder/wiki/wardriving-direct-upload) documents the credential files and saved-network behavior. SigRoam's microSD setup avoids entering the Wi-Fi password through the Flipper interface for the complete SigRoam workflow.

For either firmware, you can also power off, remove the Scout Lite microSD, and upload its WigleWifi CSV manually at [wigle.net](https://wigle.net).

## Updating firmware

Scout Lite ships with Marauder, so you can start wardriving without flashing the board. To switch between Marauder and the optional SigRoam scanner firmware, disconnect Scout Lite from the Flipper, connect it over USB-C and use the PINGEQUA web flasher:

**→ [flash.pingequa.com/devices/scout-lite](https://flash.pingequa.com/devices/scout-lite)**

Check the page for the current images and follow its BOOT-mode instructions. The two firmware choices use different flash layouts; select the matching option rather than writing one application's binary at the other's offset.

## Troubleshooting

**The Flipper GPS app shows no fix.** Almost always firmware, not hardware. Confirm the GPS UART is mapped to GPIO 15/16 (Momentum: MNTM → Protocols → GPIO Pins). Give the antenna a clear view of the sky for the first fix — a cold start can take a minute.

**No networks are logged.** Make sure a FAT32 microSD card is inserted and you started *Wardrive* (not plain Scan). Rows only get coordinates once the GPS has a fix.

**SigRoam says the serial port is busy.** Set `Settings → System → Log Device` to `Off`.

**Can I capture 5 GHz handshakes?** No — 5 GHz is scan / enumeration only. You'll see 5 GHz networks that 2.4 GHz-only boards miss, but handshake capture is a 2.4 GHz activity.

**Does it support BeiDou?** The L86-M33 covers GPS, GLONASS, and Galileo — accurate positioning worldwide. BeiDou is not required for WiGLE-grade fixes.

**Antenna.** Use the included dual-band antenna, or any SMA Wi-Fi antenna on the SMA connector. Always attach the antenna before powering on to protect the RF front-end.

## Compliance

For network research, security auditing, and education only. Contains 2.4 / 5 GHz radios — FCC Part 15 (US), ISED RSS (Canada), RED (EU / UK), MIC (Japan) apply. Firmware is ESP32 Marauder (GPLv3). The optional SigRoam Flipper app is GPL-3.0. Only scan or test networks you own or are explicitly authorized to assess.

## Links

- **Product page** — [Scout Lite on PINGEQUA](https://www.pingequa.com/products/scout-lite?utm_source=github&utm_medium=readme&utm_campaign=scout-lite)
- **Firmware flashing** — [flash.pingequa.com/devices/scout-lite](https://flash.pingequa.com/devices/scout-lite)
- **SigRoam wardriving app** — [github.com/pingequalab/sigroam-wardriving](https://github.com/pingequalab/sigroam-wardriving)
- **SigRoam scanner firmware** — [github.com/pingequalab/sigroam-firmware](https://github.com/pingequalab/sigroam-firmware)
- **Momentum firmware** — [momentum-fw.dev](https://momentum-fw.dev)
- **ESP32 Marauder** — [github.com/justcallmekoko/ESP32Marauder](https://github.com/justcallmekoko/ESP32Marauder)
- **WiGLE** — [wigle.net](https://wigle.net)
