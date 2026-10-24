# Environmental information

There are a total of 13 instructions in this category. These opcodes are used to provide information related to addresses, runtime environments, and data copy operations.

| Mnemonic | Value | POP | PUSH | Gas | Description |
|---|---|---|---|---|---|
| ADDRESS | 0x30 | 0 | 1 | 2 | Used to get the address of the currently executing account |
| BALANCE | 0x31 | 1 | 1 | 20 | Used to get the balance of the given account |
| ORIGIN | 0x32 | 0 | 1 | 2 | Used to get the address of the sender of the original transaction |
| CALLER | 0x33 | 0 | 1 | 2 | Used to get the address of the account that initiated the execution |
| CALLVALUE | 0x34 | 0 | 1 | 2 | Retrieves the value deposited by the instruction or transaction |
| CALLDATALOAD | 0x35 | 1 | 1 | 3 | Retrieves the input data that was passed a parameter with the message call |
| CALLDATASIZE | 0x36 | 0 | 1 | 2 | Used to retrieve the size of the input data passed with the message call |
| CALLDATACOPY | 0x37 | 3 | 0 | 3 | Used to copy input data passed with the message call from the current environment to the memory. |
| CODESIZE | 0x38 | 0 | 1 | 2 | Retrieves the size of running the code in the current environment |
| CODECOPY | 0x39 | 3 | 0 | 3 | Copies the running code from current environment to the memory |
| GASPRICE | 0x3a | 0 | 1 | 2 | Retrieves the gas price specified by the initiating transaction. |
| EXTCODESIZE | 0x3b | 1 | 1 | 20 | Gets the size of the specified account code |
| EXTCODECOPY | 0x3c | 4 | 0 | 20 | Used to copy the account code to the memory. |