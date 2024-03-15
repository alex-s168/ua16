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


## Documents
[base ISA specification](isa.md)
