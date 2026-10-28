# The source code

In this section, we will be using the following gateway solutions:

+   - Headquarters.Messages: This refers to the common messages for Headquarters, SiteA, and SiteB.
+   - Headquarters: This will receive messages from http://localhost:25899/Headquarter/ and http://localhost:25899/Headquarter2/, and send messages to http://localhost:25899/SiteA/ and http://localhost:25899/SiteB/.
+   - SiteA: This is a project that will receive updated price information from Headquarters across http://localhost:25899/SiteA/ and respond that it was successful to Headquarters across http://localhost:25899/Headquarter2/.
+   - SiteB: This is a project that will receive updated price information from Headquarters across http://localhost:25899/SiteB/.
+   - WebClient: This will have an Index.htm page to send a JSON script to http://localhost:25899/Headquarter/.

The preceding code solution is built using Visual Studio 2012 in Windows Server 2012, with MSMQ, DTC, NServiceBus references, and SQL Server 2012 Express LocalDB installed.

In a gateway, there are incoming channels and defined site keys to send outgoing messages to their sites. We can see in the application configuration file of the headquarters that the receiving channels for the headquarters are http://localhost:25899/Headquarter/ and http://localhost:25899/Headquarter2/.

There will be a set of site keys for sending sites that make up SiteA and SiteB.

```
<GatewayConfig>
  <Sites>
    <Site Key="SiteA" Address="http://localhost:25899/SiteA/" ChannelType="Http" />
    <Site Key="SiteB" Address="http://localhost:25899/SiteB/" ChannelType="Http" LegacyMode="false" />
  </Sites>
  <Channels>
    <Channel Address="http://localhost:25899/Headquarter/" ChannelType="Http" />
    <Channel Address="http://localhost:25899/Headquarter2/" ChannelType="Http" Default="true" />
  </Channels>
</GatewayConfig>
```