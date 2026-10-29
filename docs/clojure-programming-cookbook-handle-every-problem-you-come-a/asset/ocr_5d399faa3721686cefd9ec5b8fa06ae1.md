```
; ;=> produced a message => Hello !
; ;=> nil
; ;=> Received a message payload : Hello ! , tag : 3
```

To cancel the background thread, use the `future-cancel`.

```
(future-cancel f)
; ;=> true
```

# How it works...

AMQP stands for Advanced Message Queuing Protocol. It is an open standard for business messages between application and organization. RabbitMQ is the multiprotocol message broker and supports AMQP 0-9-1.

# Connect to broker and open channel

In the preceding example, the `simple-producer` and `simple-consumer` connect to the broker and open a channel:

```
(with-open
  [conn (core/connect {:host *host*})*])
    ch (channel/open conn)]
....
)
```

The `langohr.core/connect` connects to the broker, and the `langohr.channel/open` opens a channel for publishing and subscribing.

The `langohr.core/connect` can specify additional parameters using a map structure:

```
(core/connect {:host *host* :port 5672 :username "guest"
:password "guest" :vhost "/"})`
```

The `langohr.channel/open` takes the optional parameter of channel number to identify channels by numbers:

```
(channel/open conn 1)
```

# How producer works