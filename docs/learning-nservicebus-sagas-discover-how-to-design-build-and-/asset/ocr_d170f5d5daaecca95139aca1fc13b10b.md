The requirement of this application is that we set a timer through a GUI. This GUI will also allow some SFTP variables to be set, such as the SFTP server, username, password, and local text file location.

The timer will set a daily time in the saga to upload a daily text file, which will be given a unique name based on date and time into the SFTP server. This avoids overwriting the same filename.

The daily upload will be a simple example of uploading a file using saga interaction to a remote SFTP server, such as a bank, for batch payments. We do not wish to hardcode the timer or some of the SFTP variables. We also do not wish to use the Windows Task Manager as that will require more operational support. This exercise will be handled easily in NSB.

Separating the frontend GUI from the backend email client through a saga that is acting as a persistence and message mediator offers several benefits:

+   - **Separation of duties**: If the frontend has issues, it does not necessarily propagate those issues to the email client, as long as the messaging is correct. SFTP may have issues logging in on occasion due to password resets, or network or even server issues. By not having the frontend directly tied to the SFTP client, we can avoid the frontend crashing on SFTP issues.
+   - **Message durability**: Depending on the configuration, the messages, endpoints, and saga data are persisted to the point that the server can crash, but the data is still recoverable.
+   - **Retries**: If a message fails, it will retry several times based on the configuration.
+     This may allow us to try the SFTP client-to-server connections multiple times if there are errors within the SFTP connection.
+   - **Monitoring**: There are many tools, and depending on the persistence and queuing configurations, many ways to check the endpoints, messages, and services.
+     This may allow us to monitor the SFTP client as well as the information being passed into it to validate if it is correct for an SFTP connection.
+   - **High availability**: NSB is a high-availability framework, meaning that there are multiple connections, endpoints, and server and messaging scenarios for configuration to ensure that services are always running in the background and receiving information in a high-performance environment.

Many of these topics have been covered, but as different requirements are discussed for this sample application, these features will be added by just using NSB.