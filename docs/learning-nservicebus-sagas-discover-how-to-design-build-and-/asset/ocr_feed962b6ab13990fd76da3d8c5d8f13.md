To use ActiveMQ for NServiceBus in Visual Studio projects, the NuGet version of NServiceBus.ActiveMQ has to be installed. Visit https://www.nuget.org/packages/NServiceBus.ActiveMQ/1.0.5 on installing PM> Install-Package NServiceBus.ActiveMQ into the projects. Ensure that the configuration for the IBus is set to ActiveMQ.

```
protected void Application_Start()
{
    Configure.ScaleOut(s => s.UseSingleBrokerQueue());
    _startableBus = Configure.With()
        .DefaultBuilder()
        .RijndaelEncryptionService()
        .UseTransport<ActiveMQ>()
        .UnicastBus()
        .CreateBus();
    Configure.Instance.ForInstallationOn<Windows>().Install();

    _bus = _startableBus.Start();
    AreaRegistration.RegisterAllAreas();

    RegisterGlobalFilters(GlobalFilters.Filters);
    RegisterRoutes(RouteTable.Routes);
}
```

Ensure that the App.Config or Web.Config file have the appropriate connection string for NServiceBus/Transport to point at the correct instance of the ActiveMQ queues.

```
<connectionStrings>
<add name="NServiceBus/Transport" connectionString="ServerUrl=activemq:tcp://localhost:61616"/>
</connectionStrings>
```

Some of the EndpointConfig.cs files may not explicitly call the IBus.Configure() method, so make sure that the ActiveMQ using the transport call is explicitly called in the EndpointConfig class.