# Mouse - Device 0x90

## Ports

### 0x92 - Mouse X

A short that gives the mouse's current X position.

### 0x94 - Mouse Y

A short that gives the mouse's current Y position.

### 0x96 - Buttons

A byte that gives the current state of the mouse buttons. Each bit of the byte represents
a single mouse button, starting with the least significant bit. If a mouse button is released,
the vector will trigger and the corresponding bit is cleared.

e.x.

If mouse button 1 and mouse button 2 are being held down, the byte will be
  0b0000 0011 = 0x03

If mouse button 3 and mouse button 5 are being held down, the byte will be
  0b0001 0100 = 0x14

If mouse button 5 is released, the byte will now be
  0b0000 0100 = 0x04

### 0x9a - Scroll X

TODO

### 0x9c - Scroll Y

TODO

## Vector

The mouse vector triggers whenever:

- The mouse changes position
  - Your host's mouse position may be a fractional value or may otherwise not map 1-1
    to the varvara screen. For performance, the vector should only be triggered when
    the mouse's integer position changes.
- A mouse button is pressed
- A mouse button is released
- The scroll wheel moves
