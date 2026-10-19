Click on **Continue.**

> **Important**
> The bronze layer is the first entry of data in the lakehouse. For this reason, it is a good candidate for scanning data that may contain classified information, such as PII.

9. For **Select a scan rule set**, choose **AdlsGen2**. AdlsGen2 is the default scan rule set and includes all the supported file types for schema extraction and classification, as well as all the supported system classification rules:

# Select a scan rule set

+ New scan rule set  ↻ Refresh

Select one scan rule set to be used by your scan.

**AdlsGen2** [SYSTEM DEFAULT]
Microsoft default scan rule set that includes all supported file types for schema extraction and classification, and all supported system classification rules View detail

Continue   Back                                   Cancel

Figure 10.63 – Choosing a scan rule for the new scan

10. Click on **Continue.** On the **Set a scan trigger** screen, you can set up a scan that is recurring or happens only once:

# Set a scan trigger

Set a scan trigger to run the scan at specific dates and times. If once, the scan will start after set up is completed. If recurring, the scan will start at a date and time you choose. The initial scan is a full scan and every subsequent scan is incremental.

○ Recurring    ● Once

Continue   Back                                   Cancel

Figure 10.64 – Setting a scan trigger for the new scan