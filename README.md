# Scout Lite for Flipper Zero

**ESP32-C5 dual-band Wi-Fi board with onboard GPS.** Scout Lite ships with custom Marauder for Scout Lite already installed. The SigRoam app is the survey screen. The Marauder app is the general menu. Both apps run on that firmware. Switching apps does not reflash the board.

> For authorized network research and electronics education. Only scan or test networks you own or are explicitly authorized to assess.

## Two Flipper apps

| App | On this board |
|---|---|
| **Marauder app** | 2.4 GHz and 5 GHz scanning, wardrive with GPS, logs on the board card, WiGLE upload, and the general menu for this screenless board |
| **[SigRoam](https://github.com/pingequalab/sigroam-wardriving)** | The survey screen: live counts, GPS, the WiGLE queue, and Board tools |

SigRoam shows 2.4 GHz and 5 GHz, GPS, and how many logs are waiting to upload. WiGLE receives a log when you press Upload. The last result stays on screen. Board tools can skip a name in the SD log, delete logs, or set a split size. The ignore list does not hide a network from the air. A split size returns to 1024 KB after the board reboots.

Screens and the app install notes are in the [SigRoam README](https://github.com/pingequalab/sigroam-wardriving).

## Hardware

| | |
|---|---|
| **Wi-Fi** | ESP32-C5-WROOM-1U. 2.4 GHz and 5 GHz. Wi-Fi 6 capable. One radio, so the bands are not scanned at the same time. 5 GHz is scan and enumeration. |
| **GPS** | Quectel L86-M33, patch antenna on the board. GPS, GLONASS, Galileo. Wired to the ESP32-C5. NMEA is also on Flipper GPIO 15 and 16. |
| **Storage** | microSD on Scout Lite, FAT32. Card sold separately. |
| **Antenna** | External SMA. Dual-band antenna included. |
| **Host** | Flipper Zero GPIO header. USB-C for power and firmware update. |
| **Not included** | No CC1101. No nRF24. No Flipper Zero. |
| **On the board** | Custom Marauder for Scout Lite, pre-installed. |

## Start

1. Antenna on. Seat Scout Lite on the Flipper. FAT32 card in Scout Lite, not in the Flipper.
2. The Flipper powers the board on pin 1 and talks on pins 13 and 14. Do not power the board from pin 9 (3.3 V).
3. Use the Marauder app for scan, wardrive, and the menu. Or install SigRoam: [choose the file for your Flipper firmware](https://flash.pingequa.com/devices/flipper-sigroam), from [Releases](https://github.com/pingequalab/sigroam-wardriving/releases/latest), into `SD Card/apps/GPIO/` on the Flipper. Set `Settings → System → Log Device` to `Off`.
4. Outdoors, wait for a GPS fix, then start. Press Upload when you want the logs on WiGLE.

Wardrive on custom Marauder for Scout Lite transmits while it hops channels. A GPS fix needs open sky. Indoors, a survey may not start.

To update the board firmware, use [Marauder for Scout Lite](https://flash.pingequa.com/devices/scout-lite). That page is this custom Marauder. Switching Flipper apps does not reflash the board.

## WiGLE key and Wi-Fi

The log stays on the Scout Lite card. The Flipper app does not upload over the Flipper radio.

**WiGLE key.** Put two plain-text files in the root of the Scout Lite card. Each file holds only its value.

| File | Contents |
|---|---|
| `wigle_api_name.txt` | WiGLE API Name |
| `wigle_api_token.txt` | WiGLE API Token |

Restart the board. It reads both files at boot and deletes them. Check that they are gone. Do not post the files, or a serial log, in an issue.

**Wi-Fi.** The board does not import a network from the card. In the SigRoam app, if Upload says there is no saved Wi-Fi, press WiFi and join the network in the app. The board saves that connection and uses it when you press Upload.

You can still copy a log off the Scout Lite card and upload it at [WiGLE](https://wigle.net/).

## Links

- **Product page** — [Scout Lite on PINGEQUA](https://www.pingequa.com/products/scout-lite?utm_source=github&utm_medium=referral&utm_campaign=pq-scout-lite-c5&utm_content=scout-lite-readme)
- **Board firmware** — [Marauder for Scout Lite](https://flash.pingequa.com/devices/scout-lite)
- **SigRoam app** — [pingequalab/sigroam-wardriving](https://github.com/pingequalab/sigroam-wardriving) · [choose the file for your Flipper firmware](https://flash.pingequa.com/devices/flipper-sigroam)
