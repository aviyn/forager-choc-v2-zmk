# Forager Choc v2 ZMK config

This is the ZMK config for the Forager Choc v2 / 19 mm board: a [carrefinho/forager](https://github.com/carrefinho/forager) fork with a power switch, a reset button and 3 SK6812MINI-E status LEDs per half. It started as a copy of [forager-zmk-dongle](https://github.com/aviyn/forager-zmk-dongle), but has no dongle. The left half is the central.

The status LEDs are driven by [zmk-ledstrip-status](https://github.com/aviyn/zmk-ledstrip-status).

## Builds

GitHub Actions builds these against ZMK `main` (Zephyr 4.1, board `xiao_ble//zmk`):

- `forager_left`: central, with ZMK Studio over USB
- `forager_right`
- `settings_reset`

When coming from the dongle firmware, flash `settings_reset` to both halves first, then the new firmware, then pair the left half with the host again.

## Status LEDs

| Signal | Left | Right |
|---|---|---|
| LED data (SPIM3 MOSI) | P0.05 (D5) | P0.05 (D5) |
| LED supply enable, active low | P1.15 (D10) | P1.11 (D6) |
| Charger status (BQ25101 CHG, on the XIAO) | P0.17 | P0.17 |

The battery gauge fills from the outer LED, nearest the XIAO, on both halves. On the right half the chain runs the other way, so its overlay reverses the order.

- **Outer LED:** layer colour, left half only. It's lit while a layer key is held; a toggled layer (Num) only flashes for 1 s. Lower is blue, Raise cyan, Adjust red, Num orange, Nav green; set in `layer-colors` in `forager.dtsi`.
- **Inner LED:** caps lock, white, on both halves; caps word, magenta, left half.
- **While charging:** the battery gauge breathes the whole time.
- **On battery:** the gauge shows each time it drops a step.

On the Adjust layer, `&led_status_bat` shows the battery on both halves and `&led_status_con` shows the connection status. They're on the right-hand inner column.

## Layout

The keymap is in [`config/forager.keymap`](config/forager.keymap).
