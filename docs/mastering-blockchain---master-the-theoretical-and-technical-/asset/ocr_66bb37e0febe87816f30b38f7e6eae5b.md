# # Opcodes and their meaning

There are different opcodes that have been introduced in the EVM. Opcodes are divided into multiple categories based on the operation they perform. The list of opcodes with their meaning and usage is presented here.

# ## Arithmetic operations

All arithmetic in EVM is modulo 2^256. This group of opcodes is used to perform basic arithmetic operations. The value of these operations starts from 0x00 up to 0x0b.

| Mnemonic | Value | POP | PUSH | Gas | Description |
|---|---|---:|---:|---:|---|
| STOP | 0x00 | 0 | 0 | 0 | Halts execution |
| ADD | 0x01 | 2 | 1 | 3 | Adds two values |
| MUL | 0x02 | 2 | 1 | 5 | Multiplies two values |
| SUB | 0x03 | 2 | 1 | 3 | Subtraction operation |
| DIV | 0x04 | 2 | 1 | 5 | Integer division operation |
| SDIV | 0x05 | 2 | 1 | 5 | Signed integer division operation |
| MOD | 0x06 | 2 | 1 | 5 | Modulo remainder operation |
| SMOD | 0x07 | 2 | 1 | 5 | Signed modulo remainder operation |
| ADDMOD | 0x08 | 3 | 1 | 8 | Modulo addition operation |
| MULMOD | 0x09 | 3 | 1 | 8 | Modulo multiplication operation |
| EXP | 0x0a | 2 | 1 | 10 | Exponential operation (repeated multiplication of the base) |
| SIGNEXTEND | 0x0b | 2 | 1 | 5 | Extends the length of 2s complement signed integer |

Note that STOP is not an arithmetic operation but is categorized in this list of arithmetic operations due to the range of values (0s) it falls in.