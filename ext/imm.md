# Extension: Full Immediates
Depends on: Variable Instruction Length Extensions

This extenson adds the immediate byte and immediate word instruction, which are useful for drastically reducing program size.

## Instructions
|name|encoding|description|
|-|-|-|
|imb|001001dd bbbbbbbb|load 8-bit immediate bbbbbbbb into zero extended dd register|
|imw|001010dd wwwwwwww wwwwwwww|load 16-bit immediate wwwwwwww wwwwwwww into dd register|
