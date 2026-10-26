| 444 | Unable to route | The server was unable to determine the next hop for the message. |
| 445 | Mail system congestion | The mail server is temporarily congested. |
| 447 | Delivery time has expired | The message was considered too old by the rejecting system. This is usually due to queuing or transmission problems. |
| 450 | Requested action not taken | Message could not be transmitted. This is usually due to a problem with the mail service on the remote server. |
| 451 | Invalid command | This indicates an unsupported or out of sequence command. The action was aborted by the receiving server. This was mostly due to load on the sending or the receiving server. |
| 452 | Requested action was not taken | Insufficient storage on the receiving server. |
| 500 | Syntax error | The command sent by the server was not recognized as a valid SMTP or ESMTP command. |
| 512 | DNS error | The host server, which is the destination for the mail that was sent, could not be located. |
| 530 | Authentication problem | Authentication is required from the receiving server, or your server has been added to a black list by the receiving server. |
| 542 | Recipient address was rejected | A message indicating that your server address was rejected by the receiving server. This is usually due to Anti-spam, IDS/IPS systems, smart firewalls or other security system. |