# Scout Lite troubleshooting

[← Scout Lite overview](../README.md) · [Software and WiGLE guide](software-and-wigle.md) · [Web Flasher](https://flash.pingequa.com/devices/scout-lite)

Start with the observed symptom. Most connection, logging, and upload problems can be separated **without reflashing**.

## Five-minute preflight

1. **Power and fit:** Seat Scout Lite on the Flipper GPIO header. Attach its dual-band SMA Wi-Fi antenna before powering it.
2. **Correct storage:** Insert a FAT32 microSD card in **Scout Lite** for wardrive logs. The Flipper SD card does not replace it.
3. **Correct pair:** Check which firmware is on Scout Lite and which app is on the Flipper. Factory Marauder works with the Marauder companion or SigRoam FAP; the complete SigRoam workflow needs SigRoam scanner firmware plus matching FAP.
4. **Free serial pins:** Set Flipper **Settings → System → Log Device → Off**. In SigRoam, run **Probe firmware** if there is no response.
5. **Outdoor fix:** For GPS-tagged rows, wait for a valid GNSS fix with a clear view of the sky. A normal AP scan can still work without one.

## Symptom checklist

| Symptom | Bounded check and next step |
| --- | --- |
| **No power or no response on the Flipper** | Reseat the board and confirm the selected app. Set **Log Device → Off**; in SigRoam, run **Probe firmware**. Confirm the FAP asset matches your Flipper firmware using the [current release](https://github.com/pingequalab/sigroam-wardriving/releases/latest). |
| **Serial port busy** | The Flipper system logger may occupy pins 13/14. Set **Settings → System → Log Device → Off**, leave and reopen the app, then probe once. |
| **Scan finds APs, but no WigleWifi CSV** | Confirm a FAT32 card is in **Scout Lite** and that you selected **Wardrive**, not plain Scan. Check for a valid GNSS fix outdoors. The upstream [Marauder Wardrive guide](https://github.com/justcallmekoko/ESP32Marauder/wiki/Wardrive) documents its fix requirement. |
| **Board logs location, but native Flipper GPS app is blank** | These are separate UART paths. On compatible Momentum firmware, map NMEA GPS UART to **Extra 15,16**. This mapping is not required for Marauder or SigRoam board-side logging. |
| **SigRoam Upload shows Key or Home unconfigured** | Confirm the four upload files are in the **Scout Lite microSD root**, with exact names and one value per file. Restart the board and check that the scanner consumed them. See the [credential table](software-and-wigle.md#where-logs-and-upload-credentials-live). |
| **SigRoam survey stopped but no upload completed** | In **Upload**, inspect the queue, configured Wi-Fi and WiGLE response. Keep the board powered and card inserted. Retry pending sealed files from that page when back in Wi-Fi range. HTTP 429 is a WiGLE limit response, not success. |
| **No serial port in Web Flasher** | Disconnect Scout Lite from the Flipper. Use a supported browser and a USB-C **data** cable; then follow the [BOOT download-mode steps](https://flash.pingequa.com/devices/scout-lite). |
| **Unexpected behavior after a firmware switch** | Verify the Web Flasher image selected for Scout Lite and the FAP installed on the Flipper. Reinstall the intended **Marauder** or **SigRoam** choice from the picker, which uses the correct layout for each image. |

## Firmware recovery

Use the [Scout Lite Web Flasher](https://flash.pingequa.com/devices/scout-lite) when you actually need to switch or recover the board firmware:

1. Remove Scout Lite from the Flipper and connect it with a USB-C data cable.
2. Hold **BOOT** as you connect USB-C, then release it.
3. Choose **Marauder** to restore the factory path or **SigRoam** for the dedicated scanner, select the board's port, and finish the flash.
4. Recheck the matching Flipper app and a simple scan before diagnosing GPS or WiGLE.

The two images have different flash layouts; use the flasher's firmware choice instead of writing a standalone binary at a guessed offset. If the browser cannot see the port, the flasher's [own troubleshooting page](https://flash.pingequa.com/devices/scout-lite) is the current reference.

## What a successful check proves

- **Board responds to Probe or Marauder app:** serial connection is working at that moment; it does not prove card logging, GPS fix, or WiGLE upload.
- **APs appear during Scan:** Wi-Fi enumeration works; it does not prove a GPS-tagged CSV was written.
- **CSV exists on Scout Lite microSD:** logging worked for that session; it does not prove WiGLE accepted the file.
- **SigRoam Upload shows Uploaded with a WiGLE transId:** WiGLE accepted that file; later processing is separate.

For firmware-specific upload details, see [SigRoam Wardriving](https://github.com/pingequalab/sigroam-wardriving), [SigRoam scanner firmware](https://github.com/pingequalab/sigroam-firmware), and Marauder's [Direct Upload guide](https://github.com/justcallmekoko/ESP32Marauder/wiki/wardriving-direct-upload).
