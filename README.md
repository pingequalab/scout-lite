# Scout Lite

**Marauder-first ESP32-C5 Wi-Fi research and GPS wardriving for Flipper Zero.**

Scout Lite ships with **ESP32 Marauder**. Start with Marauder and its Flipper companion app for the broader Wi-Fi research toolkit. For a dedicated wardriving display, try the **SigRoam FAP**; for SigRoam's complete survey and WiGLE upload workflow, optionally flash its **scanner firmware** to the same board.

<p align="center">
  <img src="assets/scout-lite-on-flipper.webp" alt="Scout Lite ESP32-C5 mounted on a Flipper Zero, with a dual-band SMA antenna and the Marauder Wardrive menu on screen" width="430">
  <br><sub>Scout Lite running the factory Marauder workflow · photo: PINGEQUA</sub>
</p>

<p align="center">
  <a href="#choose-your-setup">Choose a setup</a> ·
  <a href="#start-with-marauder">Start with Marauder</a> ·
  <a href="#hardware-at-a-glance">Hardware</a> ·
  <a href="#quick-troubleshooting">Troubleshooting</a>
</p>

> **In one sentence:** Scout Lite does the Wi-Fi scanning, GNSS reading, and microSD logging; the Flipper Zero is the controller and display. The board and dual-band antenna are included. A Flipper Zero and FAT32 microSD card are not included.

## Choose your setup

### 1. Marauder firmware + Marauder app · start here

**Best for:** Wi-Fi research and the wider Marauder feature set. The ESP32-C5 Marauder image is pre-flashed on Scout Lite. Fit a microSD card, mount the board on your Flipper, and open a compatible Marauder companion app. **No board flashing is needed.** See [first survey](#start-with-marauder).

### 2. Marauder firmware + SigRoam FAP · focused wardriving display

**Best for:** Trying SigRoam's Dash / Strm / GPS / Sess field views without changing the factory image. The [SigRoam Flipper app](https://github.com/pingequalab/sigroam-wardriving) can control Marauder over its serial protocol. **Logging and upload still follow Marauder's workflow.** See the [software guide](docs/software-and-wigle.md#sigroam-fap-with-factory-marauder).

### 3. SigRoam scanner firmware + SigRoam FAP · full survey workflow

**Best for:** Passive 2.4/5 GHz survey capture, session diagnostics, sealed logs on Scout Lite's microSD, and a WiGLE upload **attempt after a successful STOP** when the configured Wi-Fi is reachable. This path requires an optional board flash and a matching Flipper FAP. See the [setup and WiGLE guide](docs/software-and-wigle.md#complete-sigroam-workflow).

> A **FAP** is installed on the **Flipper Zero**. Marauder or SigRoam **scanner firmware** is installed on **Scout Lite**. Installing the FAP alone does not enable SigRoam scanner features or its after-STOP upload.

## Start with Marauder

1. Attach the included **2.4/5 GHz SMA antenna**. Insert a FAT32 microSD card into **Scout Lite** if you want wardrive logs.
2. Seat Scout Lite on the Flipper GPIO header. It arrives with Marauder already flashed.
3. Open a compatible **Marauder companion app** on the Flipper. On Momentum, the path is **Apps → GPIO → [ESP32] WiFi Marauder**.
4. Confirm communication with a scan, then choose **Wardrive** for GPS-tagged logging. Test outdoors and wait for a valid GNSS fix. The CSV is saved to the **Scout Lite microSD**, not the Flipper SD card.

For Marauder's GPS-gated recording and its separate upload procedure, use the [official Wardrive guide](https://github.com/justcallmekoko/ESP32Marauder/wiki/Wardrive) and [Direct Upload guide](https://github.com/justcallmekoko/ESP32Marauder/wiki/wardriving-direct-upload). The [software guide](docs/software-and-wigle.md) covers FAP setup, switching firmware, and the different WiGLE paths.

## Hardware at a glance

| | |
| --- | --- |
| **Wi-Fi** | ESP32-C5-WROOM-1U; a single radio scans 2.4 GHz and 5 GHz, switching between bands. |
| **GNSS** | Quectel L86-M33 with integrated patch antenna; routed to the ESP32-C5 and to Flipper GPIO 15/16 for a separately configured native GPS app. |
| **Storage** | Onboard microSD slot for WigleWifi CSV; use FAT32. Card sold separately. |
| **Connections** | Flipper GPIO header, USB-C power/flashing, SMA connector and included dual-band antenna. |
| **Firmware** | Factory ESP32 Marauder; optional SigRoam scanner firmware via the [Scout Lite Web Flasher](https://flash.pingequa.com/devices/scout-lite). |

<p align="center">
  <img src="assets/scout-lite-components-front.webp" alt="Front of Scout Lite with L86-M33 GNSS, ESP32-C5, USB-C, BOOT button and SMA connector" width="310">
  <img src="assets/scout-lite-microsd-back.webp" alt="Back of Scout Lite with its microSD slot and Flipper GPIO pins" width="310">
  <br><sub>Front: ESP32-C5 and GNSS · Back: the board's own microSD slot · photos: PINGEQUA</sub>
</p>

**Marauder compatibility has a hardware boundary.** Scout Lite supports its published ESP32-C5 Marauder image and can restore it from the [Web Flasher](https://flash.pingequa.com/devices/scout-lite). Available functions follow the ESP32-C5 build and fitted parts: Scout Lite has no CC1101 or nRF24 radio, and **5 GHz is for scan/enumeration, not handshake capture**. It does not scan both Wi-Fi bands simultaneously.

## SigRoam for a clearer field view

The [SigRoam Wardriving FAP](https://github.com/pingequalab/sigroam-wardriving) puts AP/BLE observations, GNSS fix, and session status on the Flipper. You can try it on factory Marauder first. Flash the optional [SigRoam scanner](https://github.com/pingequalab/sigroam-firmware) only when you want its dedicated capture, sealed CSV, and post-STOP upload workflow.

<p align="center">
  <img src="assets/sigroam-dashboard.png" alt="SigRoam Flipper dashboard showing network, AP, BLE, GPS and capture status" width="340">
  <img src="assets/sigroam-upload.png" alt="SigRoam Flipper Upload view showing WiGLE upload progress" width="340">
  <br><sub>Dashboard and Upload screens · source: <a href="https://github.com/pingequalab/sigroam-wardriving/tree/main/screenshots">SigRoam Wardriving</a></sub>
</p>

SigRoam survey capture is passive: no deauthentication, handshake capture, or attack traffic. Its later WiGLE upload uses a normal Wi-Fi/HTTPS connection. Read the [software and WiGLE guide](docs/software-and-wigle.md) before switching images or placing credentials on the card.

## Quick troubleshooting

| If you see… | Check first |
| --- | --- |
| **App cannot find the board / serial busy** | Reseat Scout Lite; set Flipper **Settings → System → Log Device → Off**. In SigRoam, run **Probe firmware**. |
| **Scan works, but no wardrive CSV** | Use a FAT32 card in **Scout Lite**, select **Wardrive**, and get a valid GNSS fix outdoors. |
| **Native Flipper GPS app has no position** | Its separate NMEA input needs GPIO **15/16** mapping; board-side wardriving does not need that mapping. |
| **SigRoam upload is pending** | Check **Key**, **Home**, Wi-Fi reachability, and queue status in **Upload**; retry sealed files there. |
| **Web Flasher sees no port** | Use a USB-C **data** cable and the [BOOT download-mode sequence](https://flash.pingequa.com/devices/scout-lite). |

See the [full troubleshooting checklist](docs/troubleshooting.md) before reflashing.

## Frequently asked questions

**Does Scout Lite ship with Marauder?** Yes. ESP32 Marauder is pre-flashed and is the recommended first firmware for Wi-Fi research. The Marauder companion app is the first Flipper app to try.

**Can I use SigRoam without flashing Scout Lite?** Yes. Install the SigRoam FAP on the Flipper while Scout Lite runs factory Marauder. Marauder still owns logging and upload. The full SigRoam scanner workflow needs its separate board image.

**Where are the logs?** On the **Scout Lite microSD**. The Flipper SD card holds Flipper apps and settings. The [WiGLE guide](docs/software-and-wigle.md#where-logs-and-upload-credentials-live) explains the two upload paths.

**Can I return to Marauder?** Yes. Select **Marauder** in the [Scout Lite Web Flasher](https://flash.pingequa.com/devices/scout-lite). Use its current image picker instead of manually mixing firmware flash offsets.

## Guides and project links

- **Use and recovery:** [Software choice, FAP setup and WiGLE](docs/software-and-wigle.md) · [Troubleshooting](docs/troubleshooting.md) · [Scout Lite Web Flasher](https://flash.pingequa.com/devices/scout-lite)
- **Firmware and apps:** [ESP32 Marauder](https://github.com/justcallmekoko/ESP32Marauder) · [SigRoam FAP and releases](https://github.com/pingequalab/sigroam-wardriving) · [SigRoam scanner firmware](https://github.com/pingequalab/sigroam-firmware)
- **Product and context:** [Scout Lite product page](https://www.pingequa.com/products/scout-lite) · [PINGEQUA wardriving comparison](https://www.pingequa.com/blogs/guides-tutorials/flipper-zero-esp32-gps-wardriver-sigroam-marauder) · [Press & Media Kit](https://www.pingequa.com/pages/press-media-kit)

Photos are reused from the [PINGEQUA Scout Lite gallery](https://www.pingequa.com/products/scout-lite); FAP screens come from the [SigRoam project](https://github.com/pingequalab/sigroam-wardriving/tree/main/screenshots). Credit: PINGEQUA. Scout Lite is an independent Flipper Zero companion product; compatibility does not imply endorsement by Flipper Devices or the Marauder maintainers.

> Use only on networks you own or are authorized to assess. Follow applicable local radio requirements, including FCC Part 15 and the relevant SDoC/FCC ID boundary for the final hardware and antenna configuration.
