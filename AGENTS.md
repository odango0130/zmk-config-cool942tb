# AGENTS.md

## Project

This repository is my fork of `zmk-config-cool642tb` for customizing the cool642tb_r3 keyboard firmware.

The target keyboard is cool642tb_r3:

* ZMK-based wireless split keyboard
* Seeed Studio XIAO BLE
* Left side has a horizontal rotary encoder
* Right side has a trackball
* I am using this on Windows, mainly with Japanese IME

## Work style

Before editing files:

1. Read `docs/cool642tb_r3_context.md`.
2. Inspect the current repository files.
3. Explain the planned change briefly.
4. Then modify only the minimum necessary files.

Do not make unrelated formatting changes.

When editing keymaps:

* Prefer small, reversible changes.
* Keep the original layer structure unless explicitly asked.
* After changes, summarize:

  * which files changed
  * what behavior changed
  * how to test it on the keyboard
  * whether rebuilding firmware is required

## Safety notes

When giving flashing instructions, remind me:

* Do not connect the keyboard to USB while batteries are installed.
* Flash left and right firmware to the corresponding halves.
* Re-pair Bluetooth if settings are reset.

## Current first goal

The first goal is to make the left rotary encoder easier to test and use.

The initial firmware appears to bind the normal/default layer encoder to mouse cursor movement using `scroll_move_y`, not regular wheel scrolling.

I want to change the default layer encoder behavior from cursor movement to normal scroll, probably by replacing:

```c
sensor-bindings = <&scroll_move_y>;
```

with:

```c
sensor-bindings = <&scroll_up_down>;
```

in the `default_layer` of `config/cool642tb.keymap`.

Do not change other layers yet unless needed.
