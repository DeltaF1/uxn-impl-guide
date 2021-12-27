# Uxn device model

The Uxn CPU has 256 bytes of separate memory dedicated to interacting with I/O devices.
The CPU uses the DEI and DEO opcodes to interact with this memory in a similar manner
to the LDZ/STZ opcodes.

# Varvara

Varvara is a specification for a specific device mapping attached to a Uxn cpu core.
In Varvara's specification the device page is further subdivided into 16 discrete devices, 
each occupying 16 bytes of the page.

Each device specifies how its 16 bytes are used. A single value/variable is referred to as a "port".

The official ( somewhat terse ) specification for each device's ports is at <https://wiki.xxiivv.com/site/varvara.html>.
Check there if this documentation is missing a definition.

## Input/Output

Ports in a device's page will often be meant only for reading, or only for writing.
For example, the console device has a port for reading from stdin, as well as a port for writing out to stdout.

In a typical implementation, whenever the host language sees that there is a character waiting to be read, it triggers the console device's vector, and copies the character to the "read" port.
Similarily, when the uxn program writes to the "write" port, the host language will take this byte and write it out to the console synchronously.

## Vectors

The Varvara specification asserts that the Uxn CPU core is non-interruptible.
That is, the CPU is left to run on its current task until it hits a BRK instruction,
even if some event happens in the host system ( such as the mouse moving )
Once the CPU stops executing, the next event can be serviced.

Each [device specification](devices/) outlines the conditions under which the device vector will be triggered.
Execution begins at the device's "vector" address. If this address is equal to 0x0000, then no code is executed.
By convention, the first 2 bytes of each device store the device's vector address as a short.

The initial vector that gets run by the emulator at startup is hardcoded to be address 0x0100.