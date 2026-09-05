# Scout Lite for Flipper Zero

**Compact ESP32-C5 dual-band Wi-Fi wardriving board for the Flipper Zero.** Scan 2.4 and 5 GHz, tag every network with an onboard GPS fix, and log WiGLE-ready CSV to microSD.

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
| **Dedicated scanner firmware** | In development. Not in the flasher yet; shipping units stay on Marauder |

## Getting started

1. **Mount** Scout Lite on the Flipper Zero's GPIO header. It ships pre-flashed — nothing to install on the board.
2. **Install [Momentum](https://momentum-fw.dev)** firmware on the Flipper. Momentum bundles the Marauder companion app and can remap the GPS UART to GPIO 15/16 (stock firmware cannot).
3. **Insert a microSD card** (FAT32, not included). Logs are written here.
4. On the Flipper, open **Apps → GPIO → [ESP32] WiFi Marauder**.
5. Select **Wardrive**. Access points are logged with time, signal, channel, and GPS coordinates as WigleWifi CSV.

## Better wardriving UI — SigRoam

The board firmware stays Marauder. For a dedicated live dashboard (AP / BLE / GPS counts, unique BSSID estimate, raw serial), install **[SigRoam](https://github.com/pingequalab/sigroam-wardriving)** on the Flipper.

1. Leave the factory Marauder image on Scout Lite. **Do not flash SigRoam onto the ESP32.**
2. Download `sigroam-0.3.fap` from [Releases](https://github.com/pingequalab/sigroam-wardriving/releases/tag/v0.3). One file runs on Official and Momentum.
3. Copy it to `SD Card/apps/GPIO/` on the Flipper.
4. Set **Settings → System → Log Device** to **Off** (pins 13/14 are shared with the system log).
5. Open **Apps → GPIO → SigRoam** → Dashboard → OK to start.

WiGLE CSV is still written by Marauder to the **Scout Lite** microSD. SigRoam does not write a second log on the Flipper card.

SigRoam is receive-only: no deauth, no handshake capture, no attack features.

## GPS wiring & the Flipper GPS app

The onboard GPS is hard-wired to the ESP32-C5 (so Marauder logs location automatically), and its NMEA output is also routed to the Flipper's GPIO 15/16.

To view live position in the Flipper's native GPS app, enable the pin mapping:

> **Momentum:** MNTM → Protocols → GPIO Pins → NMEA GPS UART → **Extra 15,16**

Wardriving through Marauder works whether or not this is enabled. Stock Flipper firmware pins the GPS app to GPIO 13/14 — which the ESP32 already uses — so the native GPS app needs Momentum or Xtreme.

## Uploading logs to WiGLE

1. Power off the Flipper, remove the microSD card, and read it on a computer or phone.
2. The file is a standard `WigleWifi` CSV. Upload it at [wigle.net](https://wigle.net) or through the official WiGLE mobile app.

## Updating firmware

Scout Lite ships pre-flashed, so you can start wardriving right away. To update or re-flash ESP32 Marauder, connect the board over USB-C and use the PINGEQUA web flasher:

**→ [flash.pingequa.com/devices/scout-lite](https://flash.pingequa.com/devices/scout-lite)**

*The flashing tool is being finalized — check the page for current instructions and the latest ESP32-C5 build.*

### Coming next

A dedicated, receive-only **SigRoam scanner firmware** for Scout Lite is in development. It is not a download yet and is not on the flasher. Until it ships, leave Marauder on the board. SigRoam on the Flipper already speaks that serial protocol, so the upgrade is a browser re-flash, not a new module.

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
- **Momentum firmware** — [momentum-fw.dev](https://momentum-fw.dev)
- **ESP32 Marauder** — [github.com/justcallmekoko/ESP32Marauder](https://github.com/justcallmekoko/ESP32Marauder)
- **WiGLE** — [wigle.net](https://wigle.net)
