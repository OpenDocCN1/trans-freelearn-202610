# Logical operations

Logical operations include operations that are used to perform comparisons and Boolean logic operations. The value of these operations is in the range of 0x10 to 0x1a.

| Mnemonic | Value | POP | PUSH | Gas | Description |
|---|---|---|---|---|---|
| LT | 0x10 | 2 | 1 | 3 | Less than |
| GT | 0x11 | 2 | 1 | 3 | Greater than |
| SLT | 0x12 | 2 | 1 | 3 | Signed less than comparison |
| SGT | 0x13 | 2 | 1 | 3 | Signed greater than comparison |
| EQ | 0x14 | 2 | 1 | 3 | Equal comparison |
| ISZERO | 0x15 | 1 | 1 | 3 | Not operator |
| AND | 0x16 | 2 | 1 | 3 | Bitwise AND operation |
| OR | 0x17 | 2 | 1 | 3 | Bitwise OR operation |
| XOR | 0x18 | 2 | 1 | 3 | Bitwise exclusive OR (XOR) operation |
| NOT | 0x19 | 1 | 1 | 3 | Bitwise NOT operation |
| BYTE | 0x1a | 2 | 1 | 3 | Retrieve single byte from word |

# Cryptographic operations

There is only one operation in this category named SHA3. It is worth noting that this is not the standard SHA3 standardized by NIST but the original Keccak implementation.

| Mnemonic | Value | POP | PUSH | Gas | Description |
|---|---|---|---|---|---|
| SHA3 | 0x20 | 2 | 1 | 30 | Used to calculate Keccak 256-bit hash. |