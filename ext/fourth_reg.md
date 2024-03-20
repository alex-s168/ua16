# Extension: 4th Register
Breaks compatibility with programs that are not built for this extension specifically!
To fix, you could initialize the register with ID 11 (binary) with 1, but it is up to you if you want to do that or not.

The register with ID 11 (binary) is now a real 16-bit register.
All writes and reads to register ID 11 now correspond to that register, except for `phr` and `plr`,
which still push / pop into the program counter when register ID 11 is selected.
