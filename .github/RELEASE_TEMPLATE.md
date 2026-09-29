Marlin **2.1.2.8** (newest stable release) built for an **Ender 3** running a **BigTreeTech SKR Mini E3 V3.0** board with a **BLTouch** probe.

This carries forward the same customizations as [Marlin-2.1.2.4-Ender-3-SRK-Mini-E3-V3.0-BLTouch](https://github.com/imnotnoa/Marlin-2.1.2.4-Ender-3-SRK-Mini-E3-V3.0-BLTouch): BLTouch enabled with probe offset `{-44.5, -10, 0.00}`, bilinear bed leveling, Z-homing via probe, and the LCD/menu tweaks (mute option, turbo back button, remaining-time display, autostart menu, animated boot logo, etc.). Full source: [Marlin-2.1.2.8-Ender-3-SRK-Mini-E3-V3.0-BLTouch](https://github.com/imnotnoa/Marlin-2.1.2.8-Ender-3-SRK-Mini-E3-V3.0-BLTouch).

## What's new in this build

- **Z babystep now persists to the probe offset.** Adjusting Z babystep during a print updates the BLTouch Z-offset (`M851 Z`) directly, so `M500` afterward keeps it — no more re-dialing in the same tweak every print. The Z-offset editor also shows a graphical overlay on the LCD.
- **Babystepping is available at any time**, not just while the printer is actively moving, and the LCD shows the accumulated babystep total.
- **Smoother bed mesh.** The probed 5×5 bilinear mesh is now interpolated with 3 subdivisions per cell for better surface following between probe points.
- **`G26` mesh validation** is enabled — print a test grid on demand to visually check your leveling quality.

## ⚠️ Before you flash

- This firmware is built specifically for the **BigTreeTech SKR Mini E3 V3.0** board. Do not flash it onto any other board.
- Back up your current EEPROM settings if you can (note down your PID, E-steps, and Z-probe offset from `M503`), in case you want to compare or revert later.

## How to flash

1. Download **`firmware.bin`** from this release (below).
2. Format a microSD card as **FAT32** (8 GB or smaller is safest for compatibility).
3. Copy `firmware.bin` to the **root** of the SD card. It must be the only firmware `.bin` file on the card.
4. **Power off** the printer completely.
5. Insert the SD card into the slot **on the SKR Mini E3 V3.0 board itself** (not an LCD/screen SD slot).
6. **Power on** the printer. The onboard bootloader detects `firmware.bin` and flashes automatically — this takes a few seconds to about a minute. Don't power off during this step.
7. Once flashing finishes, power off, remove the SD card, and power back on to run normally.
8. Confirm the update by checking the boot screen, or send `M115` over USB/serial — the author string should read `(ImNotnoa, SKR-mini-E3-V3.0)`.

## After flashing

Your existing EEPROM settings should carry over fine since the probe offset and leveling type match your previous build. If you see odd behavior (wrong offsets, erratic leveling), reset to firmware defaults and re-save:

```
M502   ; load firmware defaults
M500   ; save to EEPROM
```

Then redo bed leveling (`G29`) before your next print — the mesh is now built with extra subdivisions, so a fresh probe pass is worth it. Afterward, try `G26` to print a quick validation grid and confirm the leveling looks right before committing to a full print.
