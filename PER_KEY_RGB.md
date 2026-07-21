# Per-key RGB indicators

This branch applies `patches/zmk-per-key-rgb.patch` to ZMK `v0.3.0` during the
GitHub Actions build. The patch preserves the normal underglow effects and
overlays dynamic complementary-color indicators while RGB is enabled:

- Caps Lock LED while the host reports Caps Lock as active.
- Every assigned key in layer 1, grouped by function.

Each group rotates the hue of its current pixel while preserving brightness.
This keeps indicators related to solid and animated effects instead of using a
fixed palette. A minimum saturation of 50% keeps the groups visible over white
or low-saturation effects.

| Group | Hue rotation | LED indices |
| --- | ---: | --- |
| Common keys and Caps Lock | 180 degrees | 41, 48, 56-60 |
| F1-F12 | 165 degrees | 1-12 |
| Bluetooth | 195 degrees | 14-19, 40 |
| RGB and LED power | 150 degrees | 21, 32-34, 42-46, 52 |
| Media and volume | 210 degrees | 22, 23, 31, 49-51 |
| System | 135 degrees | 53-55 |
| Editing and navigation | 225 degrees | 0, 13, 24-26, 29, 30, 47 |

Positions bound to `&none` are not highlighted. While Fn is held, its category
color takes priority over the Caps Lock indicator; therefore the Caps position
uses the Bluetooth color because it runs `BT_CLR_ALL` on layer 1.

Turning underglow off also turns the indicators off.

## Verify the LED map

The map follows the observed serpentine chain: alternating rows run in opposite
directions. Caps Lock is LED 40; LED 28 is Enter.
The added diagnostic effect lights one LED at a time in index order, changing
every 500 ms and repeating after 30.5 seconds.

To select it from the default solid effect, hold Fn and press the key that runs
`RGB_EFR` once. On the current keymap this is the `Z` position. Release Fn and
record the physical key lit for each index from 0 through 60.

If another PCB revision uses a different chain order, update `capslock-led` and
the `fn-*-leds` groups in `config/boards/shields/k65/k65.overlay` using the
observed indices.
