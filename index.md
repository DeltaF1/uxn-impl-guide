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

The first thing you should do is read <uxn.md> as well as [the Uxntal overview](https://wiki.xxiivv.com/site/uxntal.html).
These provide an overview of the guts of the virtual machine and its instructions.
<uxn.md> also contains more detail where it is lacking in the official references.
You'll need to implement a stack as well as some sort of buffer that can store 64Kb of data.

Depending on the bitwise support in your language of choice, it may be useful to create
some helper methods to translate between shorts and bytes.

### Opcodes

Once you have the basic data structures implemented, you should move on to the [Uxn instruction set reference](https://wiki.xxiivv.com/site/uxntal_reference.html).
This reference describes each of the Uxn instructions in detail with examples of what the
stack should look like for each operation.

As you implement the different opcodes, you may find it helpful to run [these test programs](https://github.com/DeltaF1/uxn-instruction-tests).
Each test program covers one set of related opcodes and should help find errors if one of
your opcodes is not implemented correctly.

By the end of this process your CPU implementation should be able to take in a
16-bit address and execute opcodes from memory until it hits a BRK opcode ( 0x00 )

### Devices


