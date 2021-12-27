# Uxn implementation guide

Uxn is a stack-based virtual machine that describes an [Instruction Set Architecture](https://en.wikipedia.org/wifi/Instruction_set_architecture).
Varvara is a specification for a set of I/O devices connected to a Uxn cpu.

<https://git.sr.ht/~rabbits/uxn> is the reference implementation of Uxn+Varvara.

## Index

### The Basics
- <uxn.md> - Information on the CPU core and instruction set
- <devices.md> - The device interrupt model of the Uxn CPU

### <devices> Varvara device spec
- <devices/console.md> - Byte stream device
- <devices/audio.md> - 8-bit sampled audio channels
- <devices/datetime.md> - System to poll the real-world time
- <devices/controller.md> - Keyboard/gamepad input device
- <devices/file.md> - Access to external data sources
- <devices/screen.md> - The graphics system.

## Porting the C code

The reference implementation uses SDL 2 and C89.

uxn.c implements the CPU core and can be copied into your project without modifications.

uxnemu.c contains SDL event handling code and window management.
It also contains all of the "glue" code to set up the CPU core and initialize memory with
ROM contents.

If your target has SDL support then the project may build on its own.
Otherwise, you will have to re-implement the [devices](devices/) and glue code
for your target platform. The device implementations are mixed between uxnemu.c and the
"device" subdirectory. 

## Porting to a new language
So you want to port Uxn to a new programming language?

The first step is to check out <uxn.md> and the [Uxn instruction set reference](https://wiki.xxiivv.com/site/uxntal_reference.html).

Your implementation should be able to take in a 16-bit address and execute until it hits
a BRK opcode ( 0x00 )

