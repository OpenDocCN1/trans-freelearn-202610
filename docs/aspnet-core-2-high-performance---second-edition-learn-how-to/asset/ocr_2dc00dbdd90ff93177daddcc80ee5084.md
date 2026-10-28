![](bbox=[0.12, 0.15, 0.2, 0.26])

For more examples of how to use logging, you can examine the default account controller (if you've included individual user account authentication in the template).

# Centralized logging

Logging is great. However, in a distributed system, you will want to feed all of your logs and exceptions into a single location, where you can easily analyze them and generate alerts. One potential option for this is Logstash (https://www.elastic.co/products/logstash), which we fleetingly mentioned in Chapter 4, Measuring Performance Bottlenecks.

If you prefer a more modular approach and want to record performance counters and metrics, then there is StatsD, which listens for UDP packets and pushes to Graphite for storage and graphing. You can get it at https://github.com/etsy/statsd and there are a few .NET clients listed on the wiki, along with a sample C# code in the main repository. Another open source option for collecting logs is Fluentd (https://www.fluentd.org/).

You may wish to use message queuing for logging so that you can quickly put a logged event into a queue and forget about it rather than directly hitting a logging server. If you directly call an API (and aren't using UDP), then make sure that it's asynchronous and non-blocking. You don't want to slow down your application by logging inefficiently.

There are also cloud options available, although the usual caveats about lock-in apply. AWS has CloudWatch, which you can read more about at https://aws.amazon.com/cloudwatch/. Azure Diagnostics is similar, and you can integrate it with Application Insights; read more at https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/azure-diagnostics.

There are other cross-platform cloud services available, such as New Relic or Stackify, but these can be quite expensive and you may wish to keep your logging within your own infrastructure. You could shoehorn the data into analytics software, such as Google Analytics or the privacy-focused Piwik (which is open source and can be self-hosted), but these are less suitable because they're designed for a slightly different purpose.