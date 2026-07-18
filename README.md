# SolarTerm
**[SolarOS](https://github.com/nilseuropa/solar_os) on the Waveshare ESP32-S3-RLCD-4.2**

## Build instructions

### Bill of materials
- A **Waveshare ESP32-S3-RLCD-4.2** board. _( Do not select a similarly named
  ESP32-S3 or e-paper board. )_
- Rii 518BT Mini BLE Keyboard
- Printable parts from this repository --> [STL](https://github.com/nilseuropa/solar_term/stl)


## First-time compile and flash

The commands below are for a terminal on Linux or macOS. Windows users can run
the same `pio` commands in a PlatformIO IDE terminal. PlatformIO's official
[installation guide](https://docs.platformio.org/en/latest/core/installation/index.html)
covers all supported installation methods.

### 1. Gather the hardware

You need:

- A USB-C cable that supports data, not a charge-only cable.
- A computer with an available USB port and internet access for the initial
  toolchain download.

The screen is fragile. Hold the PCB rather than using the display as leverage
when inserting or removing the USB-C cable. See the official
[Waveshare board documentation](https://docs.waveshare.com/ESP32-S3-RLCD-4.2)
for the connector and button locations.

### 2. Install Git, Python 3, and PlatformIO Core

Install Git and Python 3 using your operating system's package manager. Confirm
that both are available:

```sh
git --version
python3 --version
```

Install PlatformIO Core using its recommended isolated installer:

```sh
curl -fsSL -o get-platformio.py https://raw.githubusercontent.com/platformio/platformio-core-installer/master/get-platformio.py
python3 get-platformio.py
```

Make the installed `pio` command available in the current terminal and verify
it:

```sh
export PATH="$PATH:$HOME/.platformio/penv/bin"
pio --version
```

Add that `export` line to your shell profile if you want `pio` to remain on the
`PATH` in future terminal sessions. Do not run PlatformIO with `sudo`.

### 3. Download the SolarOS source

Choose a working directory, clone the repository, and enter it:

```sh
git clone https://github.com/nilseuropa/solar_os.git
cd solar_os
```

The remaining commands must be run from this directory, which contains
`platformio.ini`.

### 4. Connect and power on the board

1. Connect the board's Type-C port to the computer with the data-capable USB-C
   cable.
2. Press the board's **PWR** button once to turn it on.
3. List the serial ports detected by PlatformIO:

   ```sh
   pio device list
   ```

Note the board's port. Typical names are `/dev/ttyACM0` on Linux,
`/dev/cu.usbmodem...` on macOS, and `COM...` on Windows. In the commands below,
replace `<PORT>` with that complete name, without angle brackets.

If no new port appears, try another cable or USB port. To force the board into
download mode, long-press **PWR** to switch it off, hold **BOOT**, press **PWR**
once to switch it on, and then release **BOOT** after the USB port appears.

### 5. Compile SolarOS

Compile the firmware for the exact Waveshare target:

```sh
pio run -e waveshare_esp32_s3_rlcd_4_2
```

The first run downloads the pioarduino Espressif32 platform, the ESP-IDF
toolchain, and project dependencies, so it takes longer and requires internet
access. Continue only after PlatformIO reports `SUCCESS`.

### 6. Flash the firmware

Flashing replaces the firmware currently installed on the board. Put the board
in download mode using the **BOOT/PWR** sequence from step 4, then run:

```sh
pio run -e waveshare_esp32_s3_rlcd_4_2 -t upload --upload-port <PORT>
```

Keep the USB cable connected and do not power off the board until PlatformIO
reports `SUCCESS`. The board should reset automatically when the upload
finishes. If it remains in download mode, press **PWR** to restart it normally.

### 7. Check the first boot

SolarOS should initialize the reflective display and show its shell. To inspect
the boot log, first run `pio device list` again because the port may have
reappeared under a different name, then start the monitor:

```sh
pio device monitor --port <PORT> --baud 115200
```

A successful boot reaches `SolarOS runtime started`. Press `Ctrl+C` to leave the
monitor.

## Troubleshooting

- **No serial port:** use a known data-capable cable, connect directly instead
  of through a hub, power the board on, and repeat the **BOOT/PWR** download-mode
  sequence.
- **Permission denied on Linux:** install PlatformIO's
  [udev rules](https://docs.platformio.org/en/latest/core/installation/udev-rules.html),
  reconnect the board, and log out and back in if the instructions require a
  group change.
- **Upload cannot connect:** close every serial monitor or terminal using the
  port, force download mode again, re-run `pio device list`, and retry with the
  currently reported port.
- **A stale configuration causes a build error:** clean only this environment's
  generated build output, then compile again:

  ```sh
  pio run -e waveshare_esp32_s3_rlcd_4_2 -t clean
  pio run -e waveshare_esp32_s3_rlcd_4_2
  ```
