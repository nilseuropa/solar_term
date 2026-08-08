# SolarTerm

![SolarTerm](doc/solarterm.jpg)

SolarTerm is a handheld enclosure and expansion platform for running
[SolarOS](https://github.com/nilseuropa/solar_os) on the
[Waveshare ESP32-S3-RLCD-4.2](https://docs.waveshare.com/ESP32-S3-RLCD-4.2).
It combines the board's reflective 4.2-inch display with a Rii 518BT Mini
Bluetooth keyboard, an optional microSD card, and an 18650 battery.

> [!IMPORTANT]
> SolarTerm targets the **Waveshare ESP32-S3-RLCD-4.2**. Boards with similar
> names, including e-paper and other ESP32-S3 display boards, have different
> dimensions, connectors, and pin assignments.

## Build the enclosure

Two enclosure designs are included:

- [ATA F&E enclosure](doc/build_ata.md) — the current multipart design, with
  threaded inserts and an optional protective acrylic window.
- [Original enclosure](doc/build_nils.md) — the simpler proof-of-concept
  print-and-screw design.

Read the selected build guide before ordering or printing parts. The display is
fragile and must not be used as leverage while inserting a cable, battery, or
enclosure part.

## Install SolarOS

You need Git, Python 3, [PlatformIO Core](https://docs.platformio.org/en/latest/core/installation/index.html),
and a data-capable USB-C cable. The commands below work on Linux and macOS.
Windows users can run the same `pio` commands in a PlatformIO IDE terminal.

Do not run PlatformIO with `sudo`. If `pio` is not on your `PATH`, use the
PlatformIO virtual-environment copy directly or add it for the current shell:

```sh
export PATH="$PATH:$HOME/.platformio/penv/bin"
```

### 1. Download and build

```sh
git clone https://github.com/nilseuropa/solar_os.git
cd solar_os
pio run -e waveshare_esp32_s3_rlcd_4_2
```

The first build downloads the toolchain and dependencies. Continue only after
PlatformIO reports `SUCCESS`.

### 2. Connect the board

Connect the board's Type-C port to the computer and press **PWR** once. Hold the
PCB, not the display, while connecting the cable.

Find the serial port:

```sh
pio device list
```

Typical port names are `/dev/ttyACM0` on Linux, `/dev/cu.usbmodem...` on macOS,
and `COM...` on Windows. Substitute the complete name for `<PORT>` below.

### 3. Enter download mode and flash

1. Long-press **PWR** to turn the board off.
2. Hold **BOOT**, press **PWR** once, and release **BOOT** when the USB port
   appears.
3. Flash SolarOS:

```sh
pio run -e waveshare_esp32_s3_rlcd_4_2 -t upload --upload-port <PORT>
```

Keep the cable connected until PlatformIO reports `SUCCESS`. The board restarts
automatically. Press **PWR** once if it remains off or in download mode.

### Flashing problems

- **No serial port:** use a known data-capable cable or another USB port, then
  repeat the **BOOT/PWR** sequence.
- **Permission denied on Linux:** install PlatformIO's
  [udev rules](https://docs.platformio.org/en/latest/core/installation/udev-rules.html),
  reconnect the board, and apply any requested group change.
- **Upload cannot connect:** close programs that use the serial port, enter
  download mode again, and retry with the port currently listed by
  `pio device list`.
- **Stale build configuration:** run
  `pio run -e waveshare_esp32_s3_rlcd_4_2 -t clean`, then build again.

## First boot

1. Press **PWR** and wait for the SolarOS shell.
2. Turn on the Rii keyboard with its side switch.
3. Press the pairing button on the back of the keyboard.
4. Hold SolarTerm's **KEY** button for about two seconds. Release it when the
   keyboard icon changes to its pairing or scanning state. The display shell
   does not print a pairing message.
5. Wait for the keyboard to connect, then type at the shell.

SolarOS remembers the keyboard and reconnects on later boots. A long press of
**KEY** starts pairing again; a second long press while pairing cancels it.

## Optional microSD card

SolarTerm works without a card. To add storage, format a microSD card as FAT32
and insert it while SolarTerm is powered off. SolarOS mounts it at `/sdcard`
during boot.

```text
sd status
df
```

If the card is detected but not mounted, run `sd lsblk` and then `sd mount`.

## Update SolarOS over the air

Run `wifi` and connect to a network, then check for and install the latest
compatible SolarOS release:

```text
ota check
ota upgrade
```

SolarOS verifies the image, writes it to the inactive firmware slot, and
reboots into it. Keep the device powered until the upgrade finishes.

## Expansion cards

The Waveshare board has a 2 × 8 expansion header with 2.54 mm pitch. SolarTerm
cards use 3.3 V logic and follow the SolarOS pin assignments. See the
[SolarTerm expansion-card guide](expansions/README.md) before designing or
connecting a card.

The repository currently includes the [SolarLink radio expansion](expansions/solarlink/README.md),
with Autodesk EAGLE source, Gerber files, build information, and a photograph
of the functional prototype.

## More information

Visit [solar-os.eu](https://solar-os.eu/) for SolarOS documentation, tutorials,
and project updates.
