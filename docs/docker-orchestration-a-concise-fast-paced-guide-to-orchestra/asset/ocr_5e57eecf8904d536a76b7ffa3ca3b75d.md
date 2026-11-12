The preceding base example reads the data but does not do anything with it. In a clustered environment, it is most likely that the data will be sent to a database. The network plugin can be configured to send collected data to one or more servers. The following snippet loads the network plugin and configures collectd to send the data to two servers. The number after the IP address is the UDP port number:

```
LoadPlugin network
<Plugin network>
    Server "172.17.8.102" "25826"
    Server "172.17.8.101" "12203"
</Plugin>
```

# Storing performance data in Elasticsearch

The collectd daemon data is added to Elasticsearch through Logstash. The following example adds a UDP input plugin to the input block. Logstash will listen for collectd connections on the port specified by the port option. The buffer_size option sets the maximum packet size. For performance reasons, this should be set small enough to fit in a single network frame.

The codec option tells Logstash that the incoming packets are in binary network format of collectd. In order to properly map the data types, Logstash needs a copy of types.db from collectd. The collectd daemon usually installs types.db in /usr/share/collectd/.

This example does one last thing. It sets the type for each input plugin. This makes it easy to see only logs in queries in Kibana by adding _type:log to a query:

```
input {
  gelf {
    type => log
  }
  udp {
    port => 12203
    buffer_size => 1452
    codec => collectd {
      typesdb => [ '/usr/share/collectd/types.db' ]
    }
    type => perf
  }
}
```