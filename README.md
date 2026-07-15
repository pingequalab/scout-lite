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

## Getting started

1. **Mount** Scout Lite on the Flipper Zero's GPIO header. It ships pre-flashed — nothing to install on the board.
2. **Install [Momentum](https://momentum-fw.dev)** firmware on the Flipper. Momentum bundles the Marauder companion app and can remap the GPS UART to GPIO 15/16 (stock firmware cannot).
3. **Insert a microSD card** (FAT32, not included). Logs are written here.
4. On the Flipper, open **Apps → GPIO → [ESP32] WiFi Marauder**.
5. Select **Wardrive**. Access points are logged with time, signal, channel, and GPS coordinates as WigleWifi CSV.

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

**→ [flash.pingequa.com](https://flash.pingequa.com)**

*The flashing tool is being finalized — check the page for current instructions and the latest ESP32-C5 build.*

## Troubleshooting

**The Flipper GPS app shows no fix.** Almost always firmware, not hardware. Confirm the GPS UART is mapped to GPIO 15/16 (Momentum: MNTM → Protocols → GPIO Pins). Give the antenna a clear view of the sky for the first fix — a cold start can take a minute.

**No networks are logged.** Make sure a FAT32 microSD card is inserted and you started *Wardrive* (not plain Scan). Rows only get coordinates once the GPS has a fix.

**Can I capture 5 GHz handshakes?** No — 5 GHz is scan / enumeration only. You'll see 5 GHz networks that 2.4 GHz-only boards miss, but handshake capture is a 2.4 GHz activity.

**Does it support BeiDou?** The L86-M33 covers GPS, GLONASS, and Galileo — accurate positioning worldwide. BeiDou is not required for WiGLE-grade fixes.

**Antenna.** Use the included dual-band antenna, or any SMA Wi-Fi antenna on the SMA connector. Always attach the antenna before powering on to protect the RF front-end.

## Compliance

For network research, security auditing, and education only. Contains 2.4 / 5 GHz radios — FCC Part 15 (US), ISED RSS (Canada), RED (EU / UK), MIC (Japan) apply. Firmware is ESP32 Marauder (GPLv3). Only scan or test networks you own or are explicitly authorized to assess.

## Links

- **Product page** — [Scout Lite on PINGEQUA](https://www.pingequa.com/products/scout-lite-for-flipper-zero-esp32-c5-wi-fi-6-wardriver-with-l86-gps)
- **Firmware flashing** — [flash.pingequa.com](https://flash.pingequa.com)
- **Momentum firmware** — [momentum-fw.dev](https://momentum-fw.dev)
- **ESP32 Marauder** — [github.com/justcallmekoko/ESP32Marauder](https://github.com/justcallmekoko/ESP32Marauder)
- **WiGLE** — [wigle.net](https://wigle.net)
