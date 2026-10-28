5. Which file contains DS details on a forwarder to help establish connectivity with the DS?

+   A. `server.conf`.
+   B. `web.conf`.
+   C. `deploymentclient.conf`.
+   D. `serverclass.conf`.

6. Does the forwarder require connectivity to the indexer? If so, which statements are true about it?

+   A. No; the forwarder operates independently and does not require connectivity to the indexer.
+   B. Yes; the forwarder needs a network connection to the indexer to send data.
+   C. Yes; forwarding is enabled on forwarders through the `outputs.conf` file for indexing.
+   D. Yes; the forwarder sends data to the indexer on the default 9997 receiving port.

7. Which statement is true about the UF and HF?

+   A. The HF cannot be connected to the DS.
+   B. The UF is only compatible to work with the DS.
+   C. The HF and UF are both able to connect to the DS and get the latest apps.
+   D. Forwarders can be restarted through the DS after changes have been made through the `restartSplunkd` setting.

8. What are the ways to monitor forwarders? (Select all that apply)

+   A. By writing a script that polls the management port, 8089.
+   B. Through **Forwarder Management** in Splunk Web.
+   C. Through the MC app.
+   D. Through DS and forwarder internal logs.

9. What is the default “phone home” internal setting on forwarders?

+   A. 60 minutes.
+   B. 60 seconds.
+   C. 300 seconds.
+   D. 600 seconds.