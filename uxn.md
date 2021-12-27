# Uxn CPU core

Uxn defines some internal data structures and a set of opcodes to work upon those data structures.

## The stacks

Uxn has two 256 byte stacks, called WST ( Working Stack ) and RST ( Return Stack )

By convention the return stack is used to keep track of return addresses for subroutine calls.

By default most opcodes operate on the working stack. The JSR and STH opcodes operate
on the working stack and return stack at the same time.

## Memory

Uxn can address 64K ( 65536 bytes ) of memory.
Most programs assume that this memory is initialized to 0 before the program begins.

### Zero-page

The first 256 bytes are called the "zero-page". This region of memory can be addressed
with single byte offsets through the LDZ and STZ instructions.

## Device memory

See <devices.md> and <devices> for more information.

## Modes

Every opcode in Uxn can be modified with three different mode bits. These modes can be
combined or ommitted as desired, for a total of 256 possible operations in the VM.

### Keep mode

In "keep" mode, an opcode takes in some data from the stack as usual,
but does not update the stack pointer. This means that the result of an operation is
appended to the existing stack without taking anything off.

For example, adding two numbers would usually take two numbers off the stack, then
push the result in their place.

  a b -- a+b

In keep mode the operands are left on the stack and the result is appended as follows:

a b -- a b a+b

This can be slightly less intuitive for certain stack manipulation operations. For example,
OVRk would have the following signature:

a b -- a b a b a

### Return mode

Return mode causes the opcode to operate on the Return Stack instead of the Working Stack.
This means that the opcode will pull data from the return stack and push the results onto
the return stack.

For the opcodes that already operate on the Return stack in default mode, the stacks
they work on swap. For example, JSR2r will jump to an address off of the Return Stack,
then push the current instruction pointer to the Working stack.

### Short mode

Unless an opcode specifies that a specific operand is always a byte by ( marking it with the
caret ^  symbol ) , putting the opcode into short mode will cause all operands pulled from and
pushed to the stack to be 16-bit shorts rather than 8-bit bytes.
