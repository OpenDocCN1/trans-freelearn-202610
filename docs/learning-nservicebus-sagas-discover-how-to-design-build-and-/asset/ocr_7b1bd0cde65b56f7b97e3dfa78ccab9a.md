![](bbox=[0.18, 0.14, 0.82, 0.5])

The requirement of this application is that we set a timer through a GUI. This GUI will also allow some e-mail variables to be set as well, such as the email server, and the To and From addresses. The timer will set a time in the saga to send a daily e-mail. The daily e-mail will be a simple e-mail to send to operations the available MSMQ queues. We do not wish to hardcode the timer or any of the e-mail variables. We also do not wish to use the Windows Task Manager as that will require more operational support. This exercise will be handled easily in NSB.

Separating the frontend GUI from the backend email client through a saga that is acting as a persistence and message mediator provides several benefits:

+   - Separation of duties: If the frontend has issues, the saga does not necessarily propagate those issues to the email client, as long as the messaging is correct.
+   - Message durability: Depending on the configuration, the messages, endpoints, and saga data is persisted to the point where the server can crash, but the data is still recoverable.