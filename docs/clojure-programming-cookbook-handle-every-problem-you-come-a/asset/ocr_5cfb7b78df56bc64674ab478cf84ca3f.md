Let's walk through code to learn how producer works.

# Declaring queue

After connecting to a broker and opening a channel, producer declares an exchange to publish.

The `langohr.queue/declare` declares a queue to use and associates with a channel:

```
(queue/declare ch qname {:durable false :exclusive false :auto-delete false})
```

Mandatory parameters are a channel and a queue name. Additional parameters are the following queue properties:

+   - `:durable true`: This creates a durable queue, which enables the queue and messages in the queue survive after the event of broker reboot
+   - `:exclusive true`: This creates an exclusive queue, which can only be accessed by the current connection, and is deleted when the connection closes
+   - `:auto-delete true`: This deletes the queue after the application ends

# Producing messages

Producer produces messages to the exchange. The `langohr.basic/publish` publishes messages to exchange. Parameters are a channel, exchange type, routing-key, payload, and optional properties. In the preceding example, the default exchange type(="") is used:

```
(basic/publish
  ch "" key payload {:content-type "text/plain" :type "message"})
```

# How consumer works

The following diagram depicts an exchange and queue as examples: