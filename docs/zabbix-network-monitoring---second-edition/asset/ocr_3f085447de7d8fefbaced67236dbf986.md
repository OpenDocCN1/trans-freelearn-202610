+   - **Trigger severity**: This lets you limit messages based on trigger severity (see *Chapter 6, Detecting Problems with Triggers*, for more information on triggers). Unmarking a checkbox will not notify you about that specific severity at all. If you want to get the message but not the sound alert, choose `no_sound` from the dropdown.

> Adding new sounds is possible by copying .wav files to the audio subdirectory in the frontend directory.

Previously, when configuring frontend messaging, we set the message timeout to 180 seconds. The only reason was to give us enough time to explore the popup when it first appeared; it is not a requirement for using this feature.

Now, let's open **Monitoring | Triggers**. We should see the `CPU load too high on A test host for last 3 minutes` trigger visible with a red, flashing **PROBLEM** text in the **STATUS** column:

| SEVERITY | STATUS | INFO | LAST CHANGE ▼ | AGE | ACK | HOST | NAME |
|---|---|---|---|---|---|---|---|
| Not classified | PROBLEM | 2016-02-17 01:27:25 | 5m 54s | No 3 | A test host | CPU load too high on A test host for last 3 minutes |

> The flashing indicates that a trigger has recently changed state, which we just made it do with that increased system load.

However, if you have a new e-mail notification, you should already be aware of this state change before opening **Monitoring | Triggers**. If all went as expected, you should have received an e-mail informing you about the problem, so check your e-mail client if you haven't yet. There should be a message with the subject **PROBLEM: CPU load too high on A test host for last 3 minutes**.

Did the e-mail fail to arrive? This is most often caused by some misconfiguration in the mail delivery chain preventing the message from passing. If possible, check your e-mail server's log files as well as network connectivity and spam filters. Going to **Reports | Action log** might reveal a helpful error message.

You can stop all MD5 checksum calculation processes now with a simple *Ctrl + C*. The trigger should then change status to **OK**, though you should allow at least the configured item interval of 30 seconds to pass.

Again, check your e-mail: there should be another message, this time informing you that it's alright now, having the subject **OK: CPU load too high on A test host for last 3 minutes**.