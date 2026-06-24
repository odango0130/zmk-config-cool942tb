# cool642tb_r3 context

## What has been confirmed

This repository contains the ZMK keymap for cool642tb.

Important files:

* `config/cool642tb.keymap`
* `config/cool642tb.json`
* `config/boards/shields/cool642tb/cool642tb_L.overlay`
* `config/boards/shields/cool642tb/cool642tb_R.overlay`

## Physical / layout notes

The keyboard is split.
The left inner top position may look like a key in the logical layout, but on the actual cool642tb_r3 hardware it is a horizontal rotary encoder.

The layout has 43 key positions in the JSON layout:

* top row: 10 positions
* second row: 12 positions
* third row: 12 positions
* bottom row: 9 positions

## Initial layer behavior summary

### default layer

Normal typing is QWERTY-like.

Important bottom-row behavior:

* left Space key: tap Space, hold FUNCTION
* right Space key: tap Space, hold NUM
* Backspace exists on the bottom row
* LShift exists on the far right bottom position

### FUNCTION layer

Entered by holding the left Space key.

Used mainly for:

* numbers
* common symbols
* Muhenkan
* Delete
* volume control on rotary encoder

### NUM layer

Entered by holding the right Space key.

Used mainly for:

* F1 to F10
* arrow keys
* Japanese `kana`
* Bluetooth profile selection
* Bluetooth clear all

Bluetooth-related keys:

* `BT_SEL 0`
* `BT_SEL 1`
* `BT_SEL 2`
* `BT_CLR_ALL`

## Rotary encoder behavior

The left encoder is enabled in the left overlay with:

```c
&left_encoder {
    status = "okay";
};
```

In `config/cool642tb.keymap`, there are several encoder behavior definitions.

Important ones:

```c
scroll_up_down: behavior_sensor_rotate_mouse_wheel_up_down {
    compatible = "zmk,behavior-sensor-rotate";
    #sensor-binding-cells = <0>;
    bindings = <&msc SCRL_UP>, <&msc SCRL_DOWN>;
    tap-ms = <20>;
};
```

This appears to be normal mouse wheel scrolling.

```c
scroll_move_y: sensor_rotate_msc_for_move_y {
    compatible = "zmk,behavior-sensor-rotate";
    #sensor-binding-cells = <0>;
    bindings = <&msc MOVE_Y(20)>, <&msc MOVE_Y(-20)>;
    tap-ms = <65>;
};
```

This moves the mouse cursor vertically, not the page scroll.

The current default layer has:

```c
sensor-bindings = <&scroll_move_y>;
```

So the default rotary encoder behavior is likely cursor up/down movement, not page scrolling.

The FUNCTION layer has volume down/up on the encoder:

```c
sensor-bindings = <&inc_dec_kp C_VOL_DN C_VOL_UP>;
```

The NUM layer has PageUp/PageDown on the encoder:

```c
sensor-bindings = <&inc_dec_kp PG_UP PAGE_DOWN>;
```

## First requested change

Change only the default layer rotary encoder behavior from cursor movement to normal wheel scrolling.

Expected code change:

```diff
 default_layer {
     ...
-    sensor-bindings = <&scroll_move_y>;
+    sensor-bindings = <&scroll_up_down>;
 };
```

## Test plan after flashing

1. In normal typing mode, rotate the left encoder.

   * Expected: page scrolls up/down.
2. Hold left Space and rotate the encoder.

   * Expected: volume changes.
3. Hold right Space and rotate the encoder.

   * Expected: PageUp/PageDown.
4. If encoder still does absolutely nothing in all modes, suspect hardware, soldering, encoder enablement, or wrong firmware flashed to the wrong half.

## Japanese / English IME notes

The initial keymap has some Japanese-related keys:

* FUNCTION layer: `INT_MUHENKAN`
* NUM layer: `INT_KATAKANAHIRAGANA`
* NUM layer: `LC(SPACE)`
* NUM layer: `RG(SPACE)`

However, the initial keymap does not appear to have a clear dedicated `半角/全角` toggle key. For Windows Japanese IME, future customization may be needed.
