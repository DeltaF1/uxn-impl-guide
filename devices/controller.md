# Controller - Device 0x80

The controller device maps both an NES-style gamepad and ASCII keyboard input.

## Ports

### 0x82 - Gamepad buttons

TODO: nice glyph drawing here

### 0x83 - Keyboard key

Set to the ASCII key that was typed.
If a key was released, then set to 0x00

## Vector

The vector triggers whenever:

- One of the gamepad buttons is pressed or released
- A keyboard key is pressed
- A keyboard key is released
  - In this case, the keyboard byte will be 0x00