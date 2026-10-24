# Scout Lite for Flipper Zero

**ESP32-C5 dual-band Wi-Fi board with onboard GPS.** Custom Marauder for Scout Lite is already on the board.

Two Flipper apps use that firmware. Pick SigRoam for wardriving, or the Marauder app for Wi-Fi and Bluetooth analysis. Switching apps does not reflash the board.

**[Install the SigRoam app](https://flash.pingequa.com/devices/flipper-sigroam)** · **[Update the board](https://flash.pingequa.com/devices/scout-lite)**

> For authorized network research and electronics education. Only scan or test networks you own or are explicitly authorized to assess.

## Pick an app

Both apps run on the firmware already on the board. Switching apps does not reflash it.

### SigRoam — wardriving

Choose SigRoam for a wardrive. It shows live 2.4 GHz and 5 GHz counts, GPS, the upload queue, and Board tools while you log networks.

Install it below. Screens and the rest of the app are in the [SigRoam guide](https://github.com/pingequalab/sigroam-wardriving).

### Marauder app — Wi-Fi analysis

Choose the Marauder app for Wi-Fi and Bluetooth analysis on this screenless board: scanning, the wider Marauder menu, and logs on the board card. It can start a wardrive. SigRoam is the wardriving app.

Open `Apps → GPIO → [ESP32] WiFi Marauder`. If that item is missing, install WiFi Marauder from the Flipper app catalog.

How to use the menu is in the [Marauder getting started guide](https://github.com/justcallmekoko/ESP32Marauder/wiki/getting-started) and [Applications](https://github.com/justcallmekoko/ESP32Marauder/wiki/applications). Those pages also cover other boards. Do not flash the firmware they describe onto Scout Lite. Update this board only from [Marauder for Scout Lite](https://flash.pingequa.com/devices/scout-lite).

## Settings

1. Set `Settings → System → Log Device` to `Off` before you open either app.
2. Close qFlipper and the Flipper phone app before a browser install.
3. To upload from the board, add the WiGLE key and join a Wi-Fi network in the app you use. See [WiGLE key and Wi-Fi](#wigle-key-and-wi-fi).

## Install the SigRoam app

SigRoam is the wardriving app: live 2.4 GHz and 5 GHz counts, GPS, the upload queue, and Board tools. It goes on the Flipper, not on the ESP32.

| Dashboard | Upload |
|---|---|
| <img src="screenshots/dashboard.png" width="320" alt="SigRoam Dashboard with a live unique count, AP, BLE, 2.4 GHz, 5 GHz, GPS fix, and logs waiting to upload"> | <img src="screenshots/upload.png" width="320" alt="SigRoam Upload page with the last result, logs not yet sent, and press Upload"> |
| **Probe** | **Board tools** |
| <img src="screenshots/probe.png" width="320" alt="SigRoam Probe showing PINGEQUA Scout Lite, GPS, and the card"> | <img src="screenshots/board-tools.png" width="320" alt="SigRoam Board tools: Ignore list, Logs on card, Split size"> |

1. Open the app project: [pingequalab/sigroam-wardriving](https://github.com/pingequalab/sigroam-wardriving)
2. Install it in the browser: [flash.pingequa.com/devices/flipper-sigroam](https://flash.pingequa.com/devices/flipper-sigroam). Use Chrome, Edge, or desktop Firefox. That page picks the file for your Flipper firmware. Or copy the latest file from [Releases](https://github.com/pingequalab/sigroam-wardriving/releases/latest) to `SD Card/apps/GPIO/` on the Flipper.
3. Set `Settings → System → Log Device` to `Off`. Open `Apps → GPIO → SigRoam Wardriving`.

## Use it

1. Screw the antenna on. Power off the Flipper. Seat Scout Lite on the GPIO header.
2. Put a FAT32 card in Scout Lite, not in the Flipper. The card is not in the box.
3. Outdoors, wait for a GPS fix. In SigRoam, press OK on Dash to start, and OK again to stop. In the Marauder app, start Wardrive from the menu in the guide above.
4. Press Upload when you want the logs on WiGLE. Stopping a wardrive does not upload.

> Wardrive transmits while it hops channels. Indoors, a wardrive may not start. 5 GHz on this board is scan and enumeration.

## WiGLE key and Wi-Fi

The log stays on the Scout Lite card.

### Key

Put two plain-text files in the root of that card. Each file holds only its value.

| File | Contents |
|---|---|
| `wigle_api_name.txt` | WiGLE API Name |
| `wigle_api_token.txt` | WiGLE API Token |

Restart the board. It reads both files and deletes them. Check that they are gone.

### Wi-Fi

Wi-Fi is not read from the card. In SigRoam, if Upload says there is no saved Wi-Fi, press WiFi and join the network in the app. In the Marauder app, join the network from that app. The board saves that connection. The Marauder menu steps are in the [getting started guide](https://github.com/justcallmekoko/ESP32Marauder/wiki/getting-started).

You can also copy the CSV off the card and upload it at [WiGLE](https://wigle.net/).

## Update the board firmware

Do this only to update or restore the board. Take Scout Lite off the Flipper first.

1. Open [flash.pingequa.com/devices/scout-lite](https://flash.pingequa.com/devices/scout-lite) in Chrome, Edge, or desktop Firefox.
2. Use a USB-C data cable. Hold BOOT, plug in the cable, release BOOT, then click Connect.
3. If this board last ran the old scanner firmware, turn on Erase entire flash first.
4. When it finishes, unplug USB-C before you mount the board on the Flipper again.

The same files are on the [ESP32Marauder release](https://github.com/pingequalab/ESP32Marauder/releases/tag/scout-lite-v1.1.0). Use the flash page to install them.

## Hardware

| Item | Detail |
|---|---|
| **Wi-Fi** | ESP32-C5-WROOM-1U<br>2.4 GHz and 5 GHz. Wi-Fi 6 capable.<br>One radio, so the bands are not scanned at the same time. 5 GHz is scan and enumeration. |
| **GPS** | Quectel L86-M33, patch antenna on the board. GPS, GLONASS, Galileo. |
| **Storage** | microSD on Scout Lite, FAT32. Card sold separately. |
| **Antenna** | External SMA. Dual-band antenna included. |
| **Host** | Flipper Zero GPIO header. USB-C for power and firmware update. |
| **Not included** | No CC1101.<br>No nRF24.<br>No Flipper Zero. |
| **On the board** | Custom Marauder for Scout Lite, pre-installed. |

## Questions

<details>
<summary>The app says the serial port is busy</summary>

Set `Settings → System → Log Device` to `Off`. Pins 13 and 14 are the link to the board. Close qFlipper and the Flipper phone app before a browser install.

</details>

<details>
<summary>Nothing is logged</summary>

The card must be FAT32 and in Scout Lite, not in the Flipper. Start Wardrive, or press OK on Dash, after a GPS fix. A fix needs open sky. Indoors, a wardrive may not start.

</details>

<details>
<summary>Do I flash the app onto the board?</summary>

No. Install the app from the [SigRoam page](https://flash.pingequa.com/devices/flipper-sigroam). Update the board only from the [firmware page](https://flash.pingequa.com/devices/scout-lite).

</details>

<details>
<summary>The browser shows no port</summary>

Safari and iPhone cannot flash. Use a USB-C data cable, not a charge-only cable. Hold BOOT while you plug in, then release it and click Connect.

</details>

<details>
<summary>The menu shows two SigRoam apps</summary>

Delete the older file in `SD Card/apps/GPIO/`. The installer writes one app.

</details>

<details>
<summary>Can I capture 5 GHz handshakes?</summary>

5 GHz here is scan and enumeration. Handshake capture is a 2.4 GHz activity.

</details>

<details>
<summary>The ignore list did not hide a network</summary>

It only skips that name in the SD log. The network is still on the air. On firmware v1.1.0 or later, a split size is saved and kept after reboot. On v1.0.0, a reboot returns it to 1024 KB.

</details>

<details>
<summary>The Flipper GPS app shows no fix</summary>

Wardriving uses the GPS on Scout Lite. NMEA is also on GPIO 15 and 16. Mapping those pins is optional.

</details>

## Support

Email [support@pingequa.com](mailto:support@pingequa.com). We aim to reply within one business day, Monday–Friday. Include the order number, the Flipper firmware name, and which app you opened. Do not send the WiGLE token.

Hours and the mailing address are on the [contact page](https://www.pingequa.com/pages/contact). The full module steps are on the [Scout Lite wiki](https://www.pingequa.com/pages/wiki#scout-lite).

- **Product** — [Scout Lite on PINGEQUA](https://www.pingequa.com/products/scout-lite?utm_source=github&utm_medium=referral&utm_campaign=pq-scout-lite-c5&utm_content=scout-lite-readme)
- **SigRoam app** — [pingequalab/sigroam-wardriving](https://github.com/pingequalab/sigroam-wardriving) · [install](https://flash.pingequa.com/devices/flipper-sigroam) · [latest file](https://github.com/pingequalab/sigroam-wardriving/releases/latest)
- **Marauder app** — [getting started](https://github.com/justcallmekoko/ESP32Marauder/wiki/getting-started) · [applications](https://github.com/justcallmekoko/ESP32Marauder/wiki/applications)
- **Board firmware** — [update in the browser](https://flash.pingequa.com/devices/scout-lite) · [release files](https://github.com/pingequalab/ESP32Marauder/releases/tag/scout-lite-v1.1.0)
