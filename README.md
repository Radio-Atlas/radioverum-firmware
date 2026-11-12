# radioverum-firmware

Firmware binaries for RadioVerum boards (M5Stack StickS3). No source code.

**Latest: [v1.5.1](https://github.com/Radio-Atlas/radioverum-firmware/releases/latest)** (2026-11-12). Anyone can flash it; no key is needed.

**How to use the board: [user guide](docs/USER_GUIDE.md)** (modes, first setup, reading the screen, updates, problems).

## Which way to update

The board shows its version when it starts (e.g. `V1.4.0`).

| Your board | Do this |
|---|---|
| Shows **1.5.1 or higher** | [Update over the air](#update-over-the-air) |
| Shows **1.5.0 or lower** | [Flash over USB](#flash-over-usb), once. These versions cannot update over the air (the screen shows `UPDATE FAILED`) |
| New, or does not start | [Flash over USB](#flash-over-usb) |

## Flash over USB

You need the board, a USB-C **data** cable, and a computer with Chrome or Edge. About 5 minutes.

1. Download **[radioverum-sticks3-1.5.1-full.bin](https://github.com/Radio-Atlas/radioverum-firmware/releases/download/v1.5.1/radioverum-sticks3-1.5.1-full.bin)**. It is the only file you need.
2. Plug the board into the computer. Hold the **power button** (on the side) until the green LED blinks, then let go. The board is now in download mode; the screen may go dark.
3. Open <https://espressif.github.io/esptool-js/>. Leave Baudrate at `921600`, click **Connect**, and pick the board (`USB JTAG/serial debug unit` or `ESP32-S3`).
4. Under **Program**: Flash Address `0x0`, choose the file, click **Program**. Wait about a minute until it says done.
5. Press the power button once. The board starts and shows `V1.5.1`.
6. Set up its Wi-Fi again (flashing erases it): plug the board into an iPhone 15 or newer, a Mac or a Linux computer, open `http://192.168.7.1`, **Settings** tab, pick the network, **leave Server URL empty**, Save and reboot. With pictures: [user guide §3](docs/USER_GUIDE.md#3-first-setup).

With Python instead of a browser (replace `COM5` with your port; on macOS `/dev/cu.usbmodem…`, on Linux `/dev/ttyACM0`):

```bash
pip install esptool
python -m esptool --chip esp32s3 -p COM5 -b 921600 --before no_reset --after watchdog_reset write_flash 0x0 radioverum-sticks3-1.5.1-full.bin
```

### Keep the Wi-Fi settings (optional)

Only for a board that runs 1.4.0 or 1.5.0, and only with Python. Download [radioverum-sticks3-1.5.1.bin](https://github.com/Radio-Atlas/radioverum-firmware/releases/download/v1.5.1/radioverum-sticks3-1.5.1.bin) (the one without `-full`), put the board in download mode (step 2), then:

```bash
python -c "open('blank.bin','wb').write(b'\xff'*8192)"
python -m esptool --chip esp32s3 -p COM5 -b 921600 --before no_reset --after watchdog_reset write_flash 0xf000 blank.bin 0x20000 radioverum-sticks3-1.5.1.bin
```

The board restarts with 1.5.1 and its old settings. The first line makes an empty 8 kB file; writing it at `0xf000` makes the board start the firmware you just wrote.

## Update over the air

From 1.5.1 on. Hold the front button about 2 s: the board checks this page. If there is a newer version, press twice to install. The settings stay. A board only installs a **higher** version, and goes back to the old one by itself if the new one does not start.

## Files in a release

The two `.bin` files of a release are the **same version**, packed for two uses.

| File | What it is |
|---|---|
| `radioverum-sticks3-<version>-full.bin` | **The file you flash over USB** at `0x0` (bootloader, partition table and firmware in one) |
| `radioverum-sticks3-<version>.bin` | The update a board downloads by itself over the air (firmware only). You do not need to download it |
| `ota-sticks3.json` | The list boards read: version, file, size, SHA-256 |
| `radioverum-sticks3-<version>.enc.bin` | Up to v1.4.0: encrypted update, for boards that hold our firmware key |

## Versions

Only 1.5.1 and higher update over the air. Every older version needs one USB flash.

| Version | Date | Data on the Internet | Notes |
|---|---|---|---|
| **v1.5.1** (latest) | 2026-11-12 | not encrypted; screen `MQTT` | Fixes over-the-air updates |
| v1.5.0 | 2026-11-11 | not encrypted; screen `MQTT` | Also records the board's motion sensor (accelerometer and gyroscope, 100 Hz) next to the Wi-Fi data. Cannot update over the air |
| v1.4.0 | 2026-11-10 | not encrypted; screen `MQTT` | Plain build: no keys on the board, anyone can flash `-full.bin`. Cannot update over the air |
| v1.3.6 | 2026-11-10 | encrypted with a per-board key; screen `MQTT ENCRYPTED` / `MQTT NO KEY` | Sealed MQTT payloads. The key goes on over USB, once per board. Kept as the encrypted version |
| v1.3.5 | 2026-11-10 | not encrypted | Update check also from COLLECT; waits for Wi-Fi before checking |
| v1.3.4 | 2026-11-10 | not encrypted | First release that could update over the air |
| v1.3.2 | 2026-11-10 | not encrypted | Update check fails (`HTTP_CLIENT Out of buffer`); do not use |

Other 1.3.x numbers are not on this page.
