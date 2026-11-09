2 [https://developer.salesforce.com/docs/atlas.en-us.platform_events.meta/platform_events/platform_events_intro.htm](https://developer.salesforce.com/docs/atlas.en-us.platform_events.meta/platform_events/platform_events_intro.htm)

Change data capture³, on the other hand, is designed to capture changes in Salesforce data, allowing you to efficiently synchronize data changes with external systems. It’s worth noting that change data capture is built on the foundation of platform events, making it a specialized type of platform event tailored for data changes.

3 [https://developer.salesforce.com/docs/atlas.en-us.change_data_capture.meta/change_data_capture/cdc_intro.htm](https://developer.salesforce.com/docs/atlas.en-us.change_data_capture.meta/change_data_capture/cdc_intro.htm)

While both of these technologies provide robust solutions for integration, I won’t dive deeply into every detail. I generally lean toward using APIs for most integrations due to their versatility. However, it’s worth noting that events might be particularly suitable for logging and telemetry scenarios, especially when guaranteed delivery isn’t paramount. If you’re interested in diving deeper into these technologies, there are an abundance of references available for a comprehensive understanding.

# Challenges and limitations with platform events

Platform events come with several challenges and limitations⁴. The main consideration is that they don’t offer guaranteed delivery⁵, meaning that in scenarios where the infrastructure faces issues, events can get lost, making them unsuitable for transporting irreplaceable data. Additionally, there are significant restrictions on transports like CometD, which can hinder scaling your integration. Some users have also reported reliability concerns, noting that they missed around 5% of messages when integrating with external systems due to network hiccups. Additionally, in the context of outbound