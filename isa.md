# ua16 base ISA
## Registers
| id | name | size | description                                                                            |
| -- | ---- | ---- | -------------------------------------------------------------------------------------- |
| 00 |  R0  | 16   | general purpose                                                                        |
| 01 |  R1  | 16   | general purpose                                                                        |
| 10 |  R2  | 16   | general purpose                                                                        |
|(11)|  C1  | 16   | read only: always value 1; selected if reg id 11 and not push or pull                  |
|(11)|  PC  | 16   | programm counter (always loads from bank 0000); selected if reg id 11 and push or pull |
| -- |  BR  | 4    | bank register                                                                          |
| -- |  SP  | 8    | stack pointer (always loads from top of bank 1111)                                     |

## Instructions
(`.` means that it is ignored but should be set to zero because of future extensions)

| name | encoding | description                                                                       |
| ---- | -------- | --------------------------------------------------------------------------------- |
| adc  | 0000ddss | add with carry ss + dd into dd register; overwrites carry flag                    |
| not  | 0001ddss | bitwise not ss into dd register                                                   |
| ec9  | 0010..ss | extract 9th bit of register ss into carry flag                                    |
| fwc  | 0011..dd | fills the destination register with the carry flag                                |
| tst  | 0100..ss | carry flag = 1 if ss register zero else 0                                         |
| ltu  | 0101aabb | carry flag = unsigned **low 4 bits** of aa register less than unsigned **low 4 bits** of bb register        |
| orr  | 0110ddss | bitwise or **low 8 bits** of dd register with **low 8 bits** of ss register into zero extended dd register  |
| swp  | 0111..rr | swap the low and high byte of the rr register                                     |
| stb  | 100000rr | `rr <<= 4; rr or= bank;`                                                          |
| ldb  | 100001rr | bank = rr;                                                                        |
| clc  | 10001..0 | clear carry flag                                                                  |
| inv  | 10001..1 | invert carry flag                                                                 |
| bnc  | 1001..rr | branch to address in rr (top 4 bits of address bus are 0) if carry flag not set   |
| sbr  | 1010vvvv | set bank register to vvvv                                                         |
| phr  | 1011.0rr | push register rr onto the stack                                                   |
| plr  | 1011.1rr | pops from stack into register rr                                                  |
| ---  | 1100eeee | reserved for future use                                                           |
| mov  | 1101ddss | moves into dd register from ss register                                           |
| lod  | 1110bbaa | `b <<= 8; b or= *(char *)a;` high 4 bits of address is bank register              |
| sto  | 1111bbaa | `*(char *)b = a; a >>= 8;` high 4 bits of address is bank register                |

### Note
Instructions that overwrite the carry flag:
- adc 
- ltu
- tst
- ec9
- clc
- inv
Instructions that read the carry flag:
- fwc
- adc
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
clc
bnc temp(reg)
```

### 16 bit or
```
or dest(reg), src(reg)
swp src(reg)
swp dest(reg)
or dest(reg), src(reg)
swp src(reg)  # only if need old value of src
swp dest(reg)
```
