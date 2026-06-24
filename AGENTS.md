# AGENTS.md

## Project

This repository is my fork of `zmk-config-cool642tb` for customizing firmware for a cool642tb_r3 keyboard.

The keyboard I bought is a pre-assembled unit. The firmware currently flashed on the physical keyboard may be vendor-provided and may not match this fork exactly.

Do not assume that the current repository contents are the same as the firmware currently installed on the physical keyboard. Treat this repository as the source for building new custom firmware from now on.

## Hardware assumptions

Target keyboard:

* cool642tb_r3
* ZMK firmware
* wireless split keyboard
* Seeed Studio XIAO BLE
* left half has a horizontal rotary encoder
* right half has a trackball
* user mainly uses Windows with Japanese IME

## Work rules

Before editing:

1. Inspect the current repository files.
2. State what is confirmed from the repository.
3. Do not infer the physical keyboard's current flashed firmware unless it is directly available from files.
4. Make the minimum necessary change.
5. Do not change unrelated key layout entries.

When editing rotary encoder behavior:

* Search for `sensor-bindings`, `left_encoder`, `right_encoder`, `behavior-sensor-rotate`, `SCRL_UP`, `SCRL_DOWN`, `&msc`, and `CONFIG_ZMK_POINTING`.
* Confirm whether the left encoder is enabled in the shield overlay.
* If `sensor-bindings` is absent, add explicit sensor bindings for the desired layer.
* For the first task, change only the default layer unless explicitly asked otherwise.
* Do not assume that `scroll_move_y` or `scroll_up_down` already exists.

## First goal

Make the left rotary encoder perform normal mouse wheel scrolling on the default layer.

If the current keymap has no rotary encoder behavior, add the minimum necessary behavior definition and default-layer `sensor-bindings`.

Do not change FUNCTION, NUM, MOUSE, SCROLL, or other layers yet unless required for the build.

## Flashing safety

When providing flashing instructions, always remind me:

* Do not connect the keyboard to USB while batteries are installed.
* Flash the left firmware to the left half and the right firmware to the right half.
* Re-pair Bluetooth if settings are reset.
