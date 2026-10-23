| Flag | Meaning | SQL command | Object type |
|---|---|---|---|
| c | Connect to a database |  | Databases |
| t | Trigger or react to data changes | `CREATE TRIGGER` | Tables |
| T | Create temporary objects | `CREATE TEMP` | Tables |
| x | Cross-reference between data | `FOREIGN KEY` | Tables |
| X | Execute runnable code | `CALL` and `SELECT` | Functions, routines, and procedures |
| U | Use various objects |  | Sequences, schemas, foreign objects, types, and languages |

*Table 10.1: ACL flags*

With the list of possible flags in mind, it becomes easy to decode an ACL such as the following, which is related to a table object:

```
```text
luca=arw/enrico
```
```