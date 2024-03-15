# ua16
**extremely** minimal 16-bit processor architecture.

## Goals
- easy to implement
- easy to write programs for
- processors can be extremly simple and don't need many gates

## How
- every instruction is always one byte => no variable instruction length => extremly simple decode
- every instruction does only one thing (*a few instructions do shifts by a constant value => cost nothing to implement)

## Example implementation
### Execution stages:
- Fetch instruction (single byte)
- Perform single ALU, read or write memory operation
- Increment program counter and stack pointer if it was a stack operation

## Memory Layout
- 16 + 4 bit address space: top 4 bits are banked memory in cpu: 20 bit external address bus
- lower 16 bits are addressable by the program counter
- stack is at the begininning of the highest bank 256 bytes

## Why 16 bit?
- If the program counter is as wide as general purpose registers, we save a lot of jump instructions
- 8 bit is not enought program memory 
- 32 bit is too much for a simple processor 

## Tools
- [VXCC ua16 assembler](https://github.com/alex-s168/vxcc-v2)
- [VXCC ua16 codegen](https://github.com/alex-s168/vxcc-v2)

## Documents
[base ISA specification](isa.md)
