# Expansion cards for SolarTerm

These expansion cards fit the **Waveshare ESP32-S3-RLCD-4.2** used by
SolarTerm. Other SolarOS boards use different connectors and pin assignments.

The Waveshare host connector is a 2 × 8 female header with 2.54 mm pitch.
SolarTerm expansion cards use a 2 × 8 male header. Check the pin-1 position,
header height, and component clearance before fabrication.

## Connector

The view below matches `expansion layout` in SolarOS: component side, with the
display facing you.

| Position | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Header row 1 | VBUS | GND | GPIO19 / USB D- | GPIO20 / USB D+ | GPIO43 / TXD | GPIO44 / RXD | GPIO13 / SDA | GPIO14 / SCL |
| Header row 2 | 3V3 | GND | GPIO0 / BOOT | GPIO1 | GPIO2 | GPIO3 | GPIO17 | GPIO18 / KEY |

Check the connector orientation before soldering. A mirrored footprint connects
the wrong signals and power rails.

## SolarOS resource rules

SolarOS reserves some header pins for onboard hardware. Run these commands to
show the connector, active resources, GPIO policy, and installed drivers:

```text
expansion layout
expansion status
gpio list
expansion drivers
```

| Pins | SolarOS use on the Waveshare board |
| --- | --- |
| GPIO1, GPIO2, GPIO3, GPIO17 | Free expansion GPIO; also eligible for runtime buses, ADC, and PWM. |
| GPIO13, GPIO14 | Fixed board I2C bus `i2c0` (`SDA`, `SCL`). Use the named bus rather than claiming the pins directly. |
| GPIO43, GPIO44 | Default `uart0` (`TXD`, `RXD`). Detach `uart0` before using these pins. |
| GPIO0 | BOOT/download strapping input; fixed. |
| GPIO18 | Onboard **KEY** button; fixed. |
| GPIO19, GPIO20 | Native USB D-/D+ and USB serial; fixed. |

The Waveshare board has no fixed expansion SPI bus. SolarOS can create one on
the free pins with host `spi3`. To use GPIO43 or GPIO44, open the display shell
and detach `uart0`. SolarOS does not detach the bus that carries the current
shell.

## Electrical and mechanical rules

- Use 3.3 V logic. ESP32-S3 GPIO pins are not 5 V tolerant.
- Do not connect VBUS to 3V3 and do not back-power either rail.
- Check peak current, not only average current, before using the host's 3V3
  rail. Add local decoupling close to each load.
- Connect enough ground pins for the expected current and signal speeds.
- Do not use the BOOT, KEY, or USB pins for expansion devices. Access I2C
  devices through the fixed `i2c0` bus.
- Check connector polarity, mating height, component clearance, antenna
  clearance, and access to PWR/BOOT/KEY, USB-C, microSD, and the battery.
- Check card clearance in the selected enclosure. Add an enclosure variant when
  the card does not fit.

## Files in each card directory

Include these files:

- a card-specific `README.md` with status, target, connector orientation,
  pinout, power requirements, assembly notes, and SolarOS setup;
- editable schematic and PCB source;
- a BOM with exact manufacturer part numbers and fitted/not-fitted options;
- fabrication outputs generated from the committed PCB source;
- an assembly drawing or placement reference;
- design-rule and electrical-rule check results;
- the hardware revision and the date or tool version used to generate outputs;
- the license for the design files.

Inspect the fabrication files and test a physical board before publishing a
design.

## Included cards

| Card | Function | Status |
| --- | --- | --- |
| [SolarLink](solarlink/README.md) | Single-layer RFM69HW and RFM95W radio card | Built and validated as functional |

For the complete firmware-side model, commands, and supported drivers, see the
[SolarOS expansion documentation](https://github.com/nilseuropa/solar_os/blob/main/doc/manual/expansion.reference.md).
