# Push operations

These operations include PUSH operations that are used to place items on the stack. The range of these instructions is from 0x60 to 0x7f. There are 32 PUSH operations available in total in the EVM. PUSH operation, which reads from the byte array of the program code.

| Mnemonic | Value | POP | PUSH | Gas | Description |
|---|---|---|---|---|---|
| PUSH1 ... PUSH 32 | 0x60 ... 0x7f | 0 | 1 | 3 | Used to place N right-aligned big-endian byte item(s) on the the stack. N is a value that ranges from 1 byte to 32 bytes (full word) based on the mnemonic used. |

# Duplication operations

As the name suggests, duplication operations are used to duplicate stack items. The range of values is from 0x80 to 0x8f. There are 16 DUP instructions available in the EVM. Items placed on the stack or removed from the stack also change incrementally with the mnemonic used; for example, DUP1 removes one item from the stack and places two items on the stack, whereas DUP16 removes 16 items from the stack and places 17 items.

| Mnemonic | Value | POP | PUSH | Gas | Description |
|---|---|---|---|---|---|
| DUP1 ... DUP16 | 0x80 ... 0x8f | X | Y | 3 | Used to duplicate the nth stack item, where N is the number corresponding to the DUP instruction used. X and Y are the items removed and placed on the stack, respectively. |

# Exchange operations

SWAP operations provide the ability to exchange stack items. There are 16 SWAP instructions available and with each instruction, the stack items are removed and placed incrementally up to 17 items depending on the type of Opcode used.

| Mnemonic | Value | POP | PUSH | Gas | Description |
|---|---|---|---|---|---|
| SWAP1 ... SWAP16 | 0x90 ... 0x9f | X | Y | 3 | Used to swap the nth stack item, where N is the number corresponding to the SWAP instruction used. X and Y are the items removed and placed on the stack, respectively. |