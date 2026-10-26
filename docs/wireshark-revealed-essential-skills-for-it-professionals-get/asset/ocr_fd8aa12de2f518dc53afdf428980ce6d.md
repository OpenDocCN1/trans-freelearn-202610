+   - **Subject 5** indicates that it is a mail delivery status
+   - **Detail 0** indicates an undefined error (RFC 3463, Paragraph 3.6)

The following table lists the various classes:

| Status code | Meaning | Reason |
|---|---|---|
| 2.x.xxx | Success | Operation succeeded |
| 4.x.xxx | Persistent transient failure | A temporary condition has prevented the server from sending the message. It can be due to server load or network bottleneck. Usually, sending the message again will succeed. |
| 5.x.xxx | Permanent failure | A permanent problem prevented the server from sending the message. Usually server or compatibility errors. |

The following table lists the various subjects:

| Status code | What is it | What can be the reason |
|---|---|---|
| x.0.xxx | Other or undefined status | - |
| x.1.xxx | Addressing status | - |
| x.2.xxx | Mailbox status | - |
| x.3.xxx | Mail system status | - |
| x.4.xxx | Network and routing status | - |
| x.5.xxx | Mail delivery protocol status | - |
| x.6.xxx | Message content or media status | - |
| x.7.xxx | Security or policy status | - |

The list of status details are too long to be listed here. A full list can be found in the