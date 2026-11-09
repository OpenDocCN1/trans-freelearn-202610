can request monthly usage summaries using the **AppAnalyticsQueryRequest** object in the SOAP API from the LMO that owns the package.

# Subscriber snapshots

Subscriber snapshots provide you with a momentary overview of subscriber activity. They offer a daily snapshot of org, package, and custom entity data, recorded at 00:00 UTC and immediately available for download. You can request a snapshot for a specific date and time or a range of dates and times, receiving one snapshot per valid date and time requested.

By examining these snapshots, you can identify usage trends by org and package. This data allows you to understand the tendencies of your subscribers and make informed decisions to enhance your package. Similarly to package usage logs, snapshot data can be downloaded for 45 days from the snapshot date. Given its nature, it’s essential to capture and analyze this data promptly to understand subscriber behavior and attributes at specific moments.

There’s one more thing. Not all App Analytics data is gathered in every type of Salesforce environment. The specifics of the data collection can vary significantly based on the org type and status, as shown in the following table:

| Data type | Production | Sandbox | Scratch org | Trial |
|-----------|------------|---------|-------------|-------|
|           | Yes        | Yes     | No          | Yes   |