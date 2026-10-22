# Scout Lite for Flipper Zero

**ESP32-C5 dual-band Wi-Fi board with onboard GPS.** Custom Marauder for Scout Lite is already on the board. Install the SigRoam app on the Flipper for the survey screen. The Marauder app is the general menu. Switching apps does not reflash the board.

> For authorized network research and electronics education. Only scan or test networks you own or are explicitly authorized to assess.

## Install the SigRoam app

The app is the survey screen: live 2.4 GHz and 5 GHz counts, GPS, the upload queue, and Board tools. It goes on the Flipper, not on the ESP32.

1. Open the app project: [pingequalab/sigroam-wardriving](https://github.com/pingequalab/sigroam-wardriving)
2. Install it in the browser: [flash.pingequa.com/devices/flipper-sigroam](https://flash.pingequa.com/devices/flipper-sigroam). Use Chrome, Edge, or desktop Firefox. That page picks the file for your Flipper firmware.
3. Or copy the latest file from [Releases](https://github.com/pingequalab/sigroam-wardriving/releases/latest) to `SD Card/apps/GPIO/` on the Flipper.
4. Set `Settings → System → Log Device` to `Off`. Open `Apps → GPIO → SigRoam Wardriving`.

The Marauder app is the other path. Open `Apps → GPIO → [ESP32] WiFi Marauder`, then Wardrive. If that item is missing, install WiFi Marauder from the Flipper app catalog, or use SigRoam.

## Use it

1. Screw the antenna on. Power off the Flipper. Seat Scout Lite on the GPIO header.
2. The Flipper powers the board on pin 1 and talks on pins 13 and 14. Do not power the board from pin 9 (3.3 V).
3. Put a FAT32 card in Scout Lite, not in the Flipper. The card is not in the box.
4. Outdoors, wait for a GPS fix. In SigRoam, press OK on Dash to start, and OK again to stop.
5. Press Upload when you want the logs on WiGLE. Stopping a survey does not upload.

Wardrive transmits while it hops channels. Indoors, a survey may not start. 5 GHz on this board is scan and enumeration.

## WiGLE key and Wi-Fi

The log stays on the Scout Lite card. Put two plain-text files in the root of that card. Each file holds only its value.

| File | Contents |
|---|---|
| `wigle_api_name.txt` | WiGLE API Name |
| `wigle_api_token.txt` | WiGLE API Token |

Restart the board. It reads both files and deletes them. Check that they are gone.

Wi-Fi is not read from the card. In SigRoam, if Upload says there is no saved Wi-Fi, press WiFi and join the network in the app. The board saves that connection.

You can also copy the CSV off the card and upload it at [WiGLE](https://wigle.net/).

## Update the board firmware

Do this only to update or restore the board. Take Scout Lite off the Flipper first.

1. Open [flash.pingequa.com/devices/scout-lite](https://flash.pingequa.com/devices/scout-lite) in Chrome, Edge, or desktop Firefox.
2. Use a USB-C data cable. Hold BOOT, plug in the cable, release BOOT, then click Connect.
3. If this board last ran the old scanner firmware, turn on Erase entire flash first.
4. When it finishes, unplug USB-C before you mount the board on the Flipper again.

The same files are on the [ESP32Marauder release](https://github.com/pingequalab/ESP32Marauder/releases/tag/scout-lite-v1.0.0). Use the flash page to install them.

## Hardware

| | |
|---|---|
| **Wi-Fi** | ESP32-C5-WROOM-1U. 2.4 GHz and 5 GHz. Wi-Fi 6 capable. One radio, so the bands are not scanned at the same time. 5 GHz is scan and enumeration. |
| **GPS** | Quectel L86-M33, patch antenna on the board. GPS, GLONASS, Galileo. |
| **Storage** | microSD on Scout Lite, FAT32. Card sold separately. |
| **Antenna** | External SMA. Dual-band antenna included. |
| **Host** | Flipper Zero GPIO header. USB-C for power and firmware update. |
| **Not included** | No CC1101. No nRF24. No Flipper Zero. |
| **On the board** | Custom Marauder for Scout Lite, pre-installed. |

## Questions

**The app says the serial port is busy.**  
Set `Settings → System → Log Device` to `Off`. Pins 13 and 14 are the link to the board. Close qFlipper and the Flipper phone app before a browser install.

**Nothing is logged.**  
The card must be FAT32 and in Scout Lite, not in the Flipper. Start Wardrive, or press OK on Dash, after a GPS fix. A fix needs open sky. Indoors, a survey may not start.

**Do I flash the app onto the board?**  
No. Install the app from the [SigRoam page](https://flash.pingequa.com/devices/flipper-sigroam). Update the board only from the [firmware page](https://flash.pingequa.com/devices/scout-lite).

**The browser shows no port.**  
Safari and iPhone cannot flash. Use a USB-C data cable, not a charge-only cable. Hold BOOT while you plug in, then release it and click Connect.

**The menu shows two SigRoam apps.**  
Delete the older file in `SD Card/apps/GPIO/`. The installer writes one app.

**Can I capture 5 GHz handshakes?**  
5 GHz here is scan and enumeration. Handshake capture is a 2.4 GHz activity.

**The ignore list did not hide a network.**  
It only skips that name in the SD log. The network is still on the air. A split size returns to 1024 KB after the board reboots.

**The Flipper GPS app shows no fix.**  
Wardriving uses the GPS on Scout Lite. NMEA is also on GPIO 15 and 16. Mapping those pins is optional.

## Support

Email [support@pingequa.com](mailto:support@pingequa.com). We aim to reply within one business day, Monday–Friday. Include the order number, the Flipper firmware name, and which app you opened. Do not send the WiGLE token.

Hours and the mailing address are on the [contact page](https://www.pingequa.com/pages/contact). The full module steps are on the [Scout Lite wiki](https://www.pingequa.com/pages/wiki#scout-lite).

- **Product** — [Scout Lite on PINGEQUA](https://www.pingequa.com/products/scout-lite?utm_source=github&utm_medium=referral&utm_campaign=pq-scout-lite-c5&utm_content=scout-lite-readme)
- **SigRoam app** — [pingequalab/sigroam-wardriving](https://github.com/pingequalab/sigroam-wardriving) · [install](https://flash.pingequa.com/devices/flipper-sigroam) · [latest file](https://github.com/pingequalab/sigroam-wardriving/releases/latest)
- **Board firmware** — [update in the browser](https://flash.pingequa.com/devices/scout-lite) · [release files](https://github.com/pingequalab/ESP32Marauder/releases/tag/scout-lite-v1.0.0)
