# Per-key RGB indicators

This branch applies `patches/zmk-per-key-rgb.patch` to ZMK `v0.3.0` during the
GitHub Actions build. The patch preserves the normal underglow effects and
overlays red indicators while RGB is enabled:

- Caps Lock LED while the host reports Caps Lock as active.
- Function LEDs while layer 1 is active.

Turning underglow off also turns the indicators off.

## Verify the LED map

The initial map assumes that the 61 LEDs follow the physical keys row by row.
The added diagnostic effect lights one LED at a time in index order, changing
every 500 ms and repeating after 30.5 seconds.

To select it from the default solid effect, hold Fn and press the key that runs
`RGB_EFR` once. On the current keymap this is the `Z` position. Release Fn and
record the physical key lit for each index from 0 through 60.

If the chain order differs, update `capslock-led` and `fn-leds` in
`config/boards/shields/k65/k65.overlay` using the observed indices.
