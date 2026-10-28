For examples, we can also use the NServiceBus example for the Azure Cloud Service Bus queues from:

https://github.com/Particular/NServiceBus.Azure.Samples/tree/master/VideoStore.AzureServiceBus.Cloud

We will set up the NSB to run Azure storage queues in the following manner:

+   1. Install the reference from either NuGet or the package manager for the project for NServiceBus using PM> Install-Package NServiceBus.Azure.Transports.WindowsAzureServiceBus.

> You may need to match the compatible versions of NServiceBus core versions with the corresponding NServiceBus Azure versions. One thought is to uninstall the current NServiceBus references, and let the NServiceBus Azure references install the required NServiceBus core references.

+   2. Set up the configuration on the IBus configurations. In the MySaga project and EndpointConfig.cs file, we use the AzureServiceBus for the queue as follows:

```
Configure.With()
.DefaultBuilder()  // Autofac Default Container
.UseTransport<NServiceBus.AzureServiceBus>()  // Azure Storage Queues
.AzureSubscriptionStorage()
.AzureSagaPersister()
.UseAzureTimeoutPersister()
.UnicastBus(); // Create the default unicast Bus
```