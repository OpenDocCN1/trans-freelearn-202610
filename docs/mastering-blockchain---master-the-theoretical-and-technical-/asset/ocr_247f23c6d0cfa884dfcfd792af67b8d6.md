# Logging operations

Logging operations provide opcodes to append log entries on the sub-state tuple's log series field. There are four log operations available in total and they range from value 0x0a to 0xa4.

| Mnemonic | Value | POP | PUSH | Gas | Description |
|---|---|---|---|---|---|
| LOG0 ... LOG4 | 0x0a ... 0xa4 | X | Y (0) | 375, 750, 1125, 1500, 1875 | Used to append log record with N topics, where N is the number corresponding to the LOG Opcode used. For example, LOG0 means a log record with no topics, and LOG4 means a log record with four topics. X and Y represent the items removed and placed on the stack, respectively. X and Y change incrementally, starting from 2, 0 up to 6, 0 according to the LOG operation used. |

# System operations

System operations are used to perform various system-related operations, such as account creation, message calling, and execution control. There are six Opcodes available in total in this category.

| Mnemonic | Value | POP | PUSH | Gas | Description |
|---|---|---|---|---|---|
| CREATE | 0xf0 | 3 | 1 | 32000 | Used to create a new account with the associated code. |
| CALL | 0xf1 | 7 | 1 | 40 | Used to initiate a message call into an account. |
| CALLCODE | 0xf2 | 7 | 1 | 40 | Used to initiate a message call into this account with an alternative account's code. |
| RETURN | 0xf3 | 2 | 0 | 0 | Stops the execution and returns output data. |
| DELEGATECALL | 0xf4 | 6 | 1 | 40 | The same as CALLCODE but does not change the current values of the sender and the value. |
| SUICIDE | 0xff | 1 | 0 | 0 | Stops (halts) the execution and the account is registered for deletion later |

In this section, all EVM opcodes have been discussed. There are 129 opcodes available in the EVM of the homestead release of Ethereum in total.