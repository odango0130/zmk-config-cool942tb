# cool642tb_r3 context

## Important premise

The physical keyboard is a pre-assembled cool642tb_r3.

The firmware currently flashed on the physical keyboard may be vendor-provided and may not match this fork exactly.

Therefore, do not treat this repository as proof of the physical keyboard's current firmware. This repository is the source for building new custom firmware from now on.

## Current repository observation

The currently inspected `config/cool642tb.keymap` has these layers:

* `default_layer`
* `FUNCTION`
* `NUM`
* `ARROW`
* `MOUSE`
* `SCROLL`
* `layer_6`

The pasted `keymap` does not contain `sensor-bindings`.

Therefore, for firmware built from this repository, rotary encoder rotation behavior may not be explicitly defined unless it is defined elsewhere in the repository.

Before editing, search the repository for:

* `sensor-bindings`
* `left_encoder`
* `right_encoder`
* `behavior-sensor-rotate`
* `SCRL_UP`
* `SCRL_DOWN`
* `&msc`
* `CONFIG_ZMK_POINTING`

## Current keymap behavior summary from pasted file

### default layer

Normal typing is QWERTY-like.

Important default-layer keys:

* `Esc`, `Tab`, `LShift`, `RShift`
* `Delete`, `Backspace`, `Enter`
* left thumb: `LCTRL`, `LGUI`, `LALT`, `lt 1 SPACE`
* right thumb: `lt 2 SPACE`

The `lt 1 SPACE` key means:

* tap: Space
* hold: layer 1, likely FUNCTION

The `lt 2 SPACE` key means:

* tap: Space
* hold: layer 2, likely NUM

### FUNCTION layer

Contains:

* Bluetooth profile selection keys:

  * `BT_SEL 0`
  * `BT_SEL 1`
  * `BT_SEL 2`
* number keys
* common symbols
* Delete / Backspace / Enter

### NUM layer

Contains:

* F1 to F10
* arrow keys
* `LC(SPACE)`
* `RG(SPACE)`
* braces and symbols

### Combos

The pasted file includes combos such as:

* `clear_ble_setting`: `BT_CLR_ALL` on layer 1
* `shift_tab`: momentary layer 1 on layer 0
* `muhennkann`: macro using `INT_MUHENKAN`
* `double_quotation`
* `eq`

The exact key positions should be verified against the keyboard layout before changing combos.

## Rotary encoder goal

The first customization goal is:

Make the left horizontal rotary encoder perform normal mouse wheel scrolling on the default layer.

Because the pasted `keymap` does not contain `sensor-bindings`, the likely implementation is to add:

1. a rotary behavior for normal scroll, using `SCRL_UP` and `SCRL_DOWN`
2. a `sensor-bindings` entry in `default_layer`

Possible behavior definition:

```c
scroll_up_down: behavior_sensor_rotate_mouse_wheel_up_down {
    compatible = "zmk,behavior-sensor-rotate";
    #sensor-binding-cells = <0>;
    bindings = <&msc SCRL_UP>, <&msc SCRL_DOWN>;
    tap-ms = <20>;
};
```

Possible default layer addition:

```c
sensor-bindings = <&scroll_up_down>;
```

Do not assume `scroll_move_y` exists. Do not replace `scroll_move_y` unless the current repository actually contains it.

## Things to verify before finalizing

* Whether `&msc` is available and pointing support is enabled.
* Whether `CONFIG_ZMK_POINTING=y` exists in an appropriate `.conf` file.
* Whether the left encoder is enabled in the left shield overlay.
* Whether this keyboard uses one sensor or multiple sensors.
* Whether `sensor-bindings = <...>;` should be added to only `default_layer` or also other layers later.

## Test plan after flashing

1. In normal/default layer, rotate the left encoder.

   * Expected: page scrolls up/down.
2. Confirm normal typing still works.
3. Confirm left Space hold still enters FUNCTION.
4. Confirm right Space hold still enters NUM.
5. If the encoder still does nothing, investigate:

   * wrong firmware flashed to the wrong half
   * encoder not enabled in overlay
   * pointing/mouse config missing
   * hardware or soldering issue
   * physical encoder failure
