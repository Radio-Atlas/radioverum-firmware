# radioverum-firmware

Firmware binaries for RadioVerum boards (M5Stack StickS3). No source code.

**Latest: [v1.4.0](https://github.com/Radio-Atlas/radioverum-firmware/releases/latest)**. Anyone can flash it; no key is needed.

**How to use the board: [user guide](docs/USER_GUIDE.md)** (modes, first setup, reading the screen, updates, problems).

## Flash a board (USB)

For a new board, or one that no longer starts. You need the board, a USB-C **data** cable, and a computer with Chrome or Edge.

1. Download `radioverum-sticks3-<version>-full.bin` from the [latest release](https://github.com/Radio-Atlas/radioverum-firmware/releases/latest).
2. Plug the board into the computer. Hold the **power button** (on the side) until the green LED blinks, then let go. The board is now in download mode; the screen may go dark.
3. Open <https://espressif.github.io/esptool-js/>. Leave Baudrate at `921600`, click **Connect**, and pick the board (`USB JTAG/serial debug unit` or `ESP32-S3`).
4. Under **Program**: Flash Address `0x0`, choose the `-full.bin` file, click **Program**. Wait about a minute until it says done.
5. Press the power button once. The board starts and shows the version.
6. Set up its Wi-Fi again (flashing erases it): plug the board into an iPhone 15 or newer, a Mac or a Linux computer, open `http://192.168.7.1`, **Settings** tab, pick the network, **leave Server URL empty**, Save and reboot. With pictures: [user guide §3](docs/USER_GUIDE.md#3-first-setup).

With Python instead of a browser (replace `COM5` with your port; on macOS `/dev/cu.usbmodem…`, on Linux `/dev/ttyACM0`):

```bash
pip install esptool
python -m esptool --chip esp32s3 -p COM5 -b 921600 --before no_reset --after watchdog_reset write_flash 0x0 radioverum-sticks3-1.4.0-full.bin
```

## Update over the air

On the board, hold the front button about 2 s: it checks this page (`releases/latest/download/ota-sticks3.json`). If there is a newer version, press twice to install. A board only installs a **higher** version, and goes back to the old one by itself if the new one does not start.

## Files in a release

| File | What it is |
|---|---|
| `radioverum-sticks3-<version>-full.bin` | everything for a USB flash at `0x0` (bootloader, partition table, app). From v1.4.0 |
| `ota-sticks3.json` | the manifest boards read: version, file, size, SHA-256, `encrypted` |
| `radioverum-sticks3-<version>.enc.bin` | OTA image, encrypted: only boards holding our firmware key can install it |
| `radioverum-sticks3-<version>.bin` | OTA image, plain (from v1.4.1) |

## Versions

| Version | Date | Data on the Internet | OTA image | Notes |
|---|---|---|---|---|
| **v1.4.0** (latest) | 2026-11-10 | **not encrypted**; screen `MQTT` | encrypted copy, the last one | Plain build: no keys on the board, anyone can flash `-full.bin`. The encrypted OTA copy lets boards on 1.3.x that hold our firmware key update over the air; boards without the key flash `-full.bin` over USB. From v1.4.1 updates are plain |
| v1.3.6 | 2026-11-10 | encrypted with a per-board key; screen `MQTT ENCRYPTED` / `MQTT NO KEY` | encrypted | Sealed MQTT payloads. The key goes on over USB, once per board. Kept as the encrypted version |
| v1.3.5 | 2026-11-10 | not encrypted | encrypted | Update check also from COLLECT; waits for Wi-Fi before checking |
| v1.3.4 | 2026-11-10 | not encrypted | encrypted | First good OTA release (works with GitHub's long download links) |
| v1.3.2 | 2026-11-10 | not encrypted | encrypted | Update check fails (`HTTP_CLIENT Out of buffer`); do not use |

Other 1.3.x numbers are not on this page.
