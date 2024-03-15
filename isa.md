# ua16 base ISA
## Registers
 00   R0  16  general purpose
 01   R1  16  general purpose
 10   R2  16  general purpose
(11)  C1  16  read only: always value 1; selected if reg id 11 and not push or pull
(11)  PC  16  programm counter (always loads from bank 0000); selected if reg id 11 and push or pull
 --   BR  4   bank register
 --   SP  8   stack pointer (always loads from top of bank 1111)

## Instructions
adc  0000ddss  add with carry ss + dd into dd register; overwrites carry flag
sbc  0001ddss  subtract with carry dd - ss into dd register; overwrites carry flag
ec9  0010..ss  extract 9th bit of register ss into carry flag
fwc  0011..dd  fills the destination register with the carry flag
tst  0100..ss  carry flag = 1 if ss register zero else 0
ltu  0101aabb  carry flag = 1 if aa register less than (unsigned) bb register else 0
orr  0110ddss  bitwise or dd register | ss into dd register
clc  0111...0  clear carry flag
inv  0111...1  invert carry flag
stb  1000.0rr  rr <<= 4; rr |= bank;
ldb  1000.1rr  bank = rr;
bnc  1001..rr  branch to address in rr (top 4 bits of address bus are 0) if carry flag not set
sbr  1010vvvv  set branch register to vvvv
phr  1011.0rr  push register rr onto the stack
plr  1011.1rr  pops from stack into register rr
---  1100eeee  reserved
mov  1101ddss  moves into dd register from ss
lod  1110bbaa  b <<= 8; b |= *(char *)a; high 4 bits of address is bank register
sto  1111bbaa  *(char *)b = a; a >>= 8; high 4 bits of address is bank register

### Note
Instructions that overwrite the carry flag:
- adc
- sbc
- ltu
- tst
- ec9
- clc
- inv
Instructions that read the carry flag:
- fwc
- adc
- sbc
- bnc
- inv

## Special rules
- `phr pc` pushes the value of the program counter at this instruction.
- moves into read only register c1 are considerered undefined behaviour and should not be performed.

## Common operations
### Immediate 8 bit value
```
sbr value(imm)&0xF
stb dest(reg)
sbr value(imm)>>4
stb dest(reg)
```

### Call function
```
phr pc
clc
bnc inst(reg)
```

### Return from function
```
plr temp(reg)
add temp(reg), 1
add temp(reg), 1
bnc temp(reg)
```
