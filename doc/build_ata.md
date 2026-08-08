# ATA F&E SolarTerm enclosure

This enclosure uses threaded inserts and a separate bezel. It can also accept
an optional 2 mm acrylic display window.

## Parts

- 1 × **Waveshare ESP32-S3-RLCD-4.2** board
- 1 × Rii 518BT Mini Bluetooth keyboard
- 1 × 18650 lithium-ion battery suitable for the Waveshare holder
- 1 × microSD card, optional but recommended
- 1 × speaker with the connector supplied for the Waveshare board
- 4 × original screws supplied with the Waveshare kit
- 4 × M3 × 5 mm screws
- 4 × M3 heat-set inserts
- [Bezel](../stl/ata/Bezel.stl)
- [Case back](../stl/ata/Caseback.stl), or the
  [left-hanger case back](../stl/ata/Caseback_left.stl)
- [Button covers](../stl/ata/Buttons.stl)
- [2 mm acrylic window template](../stl/ata/plexiglass.dxf), optional

A printable [screen protector](../stl/ata/screensaver.stl) is also included for
storage and transport. It is not installed during normal use.

Use only the Waveshare ESP32-S3-RLCD-4.2. Other Waveshare ESP32-S3 and e-paper
boards do not fit this enclosure.

## Assembly

The RLCD panel is fragile. Work on a clean, soft surface, do not press on the
display, and do not overtighten the screws.

### 1. Print the enclosure parts

Print the bezel, one case-back variant, and the button covers.

![Printed parts](<ata/image_(9).png>)

### 2. Install the threaded inserts

Heat-set the four M3 inserts into the case back. Keep them straight and flush
with the plastic.

![Installing an insert](<ata/image_(8).png>)
![Flush threaded insert](<ata/image_(7).png>)

### 3. Install the speaker and buttons

Attach the self-adhesive speaker in its recess and place the button covers in
the case back.

![Speaker and buttons installed](<ata/image_(6).png>)

### 4. Connect the speaker and place the display assembly

Connect the speaker before seating the Waveshare board. Route the cable so that
the board and enclosure cannot pinch it.

![Display positioned in case back](<ata/image_(5).png>)

### 5. Secure the Waveshare board

Use the four original Waveshare screws. Tighten them evenly and only until the
board is secure.

![Board secured in case back](<ata/image_(4).png>)

### 6. Fit the bezel

If used, place the 2 mm acrylic window between the display and bezel. Insert the
keyboard, then fasten the bezel with the four M3 × 5 mm screws. The keyboard can
also be slid into place afterward if the print tolerance permits it.

![Bezel assembly](<ata/image_(3).png>)

### 7. Install the battery and SolarOS

Insert the battery with the polarity shown on the Waveshare holder, then follow
the [SolarOS installation instructions](../README.md#install-solaros).

![Battery installed](<ata/image_(2).png>)

### 8. Power on

Check that the buttons move freely and that no cable is trapped before pressing
**PWR**.

![Completed SolarTerm](<ata/image_(1).png>)
