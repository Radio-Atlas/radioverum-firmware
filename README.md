# RadioVerum firmware

Firmware for RadioVerum boards (M5Stack StickS3).

**Latest version: 1.5.4**

## Put the latest firmware on a board

About 5 minutes. You need the board, a USB-C **data** cable, and a computer with Chrome or Edge. No account, no key, nothing to install.

1. **Download the firmware:** [radioverum-sticks3-1.5.4-full.bin](https://github.com/Radio-Atlas/radioverum-firmware/releases/download/v1.5.4/radioverum-sticks3-1.5.4-full.bin)
2. **Put the board in download mode.** Plug it into the computer. Hold the **power button** (on the side) until the green LED blinks, then let go. The screen may go dark; that is normal.
3. **Connect.** Open <https://espressif.github.io/esptool-js/> and click **Connect**. Pick the board in the list (`USB JTAG/serial debug unit` or `ESP32-S3`).
4. **Write the firmware.** Change **Flash Address** from `0x1000` to **`0x0`**, choose the file from step 1, and click **Program**. Wait about a minute, until the black box shows `Leaving...`.
5. **Start the board.** Press the power button once. The screen shows `V1.5.4`.
6. **Set up Wi-Fi.** Flashing erased it. Follow the [first setup](docs/USER_GUIDE.md#3-first-setup) in the user guide.

If it does not work:

| Problem | What to do |
|---|---|
| No board in the list at step 3 | Use another cable (some only charge), and hold the power button again until the green LED blinks |
| Screen stays dark after step 4 | Press the power button once |

How to use the board afterwards: [user guide](docs/USER_GUIDE.md).

## Later updates need no cable

From 1.5.1 on, the board updates itself over Wi-Fi: hold the front button about 2 s. If the screen says `NEW VERSION`, press twice. Your settings stay.

A board that shows 1.5.0 or lower cannot do this (the screen shows `UPDATE FAILED`). Flash it once with the steps above.

## Which file do I need?

Only the one in step 1. The other files on the [release page](https://github.com/Radio-Atlas/radioverum-firmware/releases/latest) are not a second version.

| File | Do you need it? |
|---|---|
| `radioverum-sticks3-1.5.4-full.bin` | **Yes.** The file you flash |
| `radioverum-sticks3-1.5.4.bin`, `ota-sticks3.json` | No. Boards download these by themselves when they update over Wi-Fi |
| Source code (zip, tar.gz) | No. Added by GitHub; it holds only this documentation |

## Versions

| Version | Date | What changed |
|---|---|---|
| **1.5.4** (latest) | 2026-11-12 | Keeps the server connection when the board changes access point; less upload |
| 1.5.1 | 2026-11-12 | Updates over Wi-Fi work again |
| 1.5.0 | 2026-11-11 | The board also records its own motion (accelerometer and gyroscope). Cannot update over Wi-Fi |
| 1.4.0 | 2026-11-10 | First version anyone can flash, no key needed. Cannot update over Wi-Fi |
| 1.3.2 to 1.3.6 | 2026-11-10 | Early versions |
