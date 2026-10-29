messages, mosquitto_sub.

The command works the same as the pub version, except we do not set the message parameter:

```
mosquitto_sub -h localhost -t topic/hello
```

The script starts and outputs nothing yet, but let's resend the same message for more coffee because the waiter did not get it the first time:

```
Nicolass-MacBook:chapter04 niko$ mosquitto_sub -h localhost -t topic/hello
one more cup of coffee?
```

Now you can see that our running subcommand has retrieved the message properly. And you can also see that the MQTT spy window is displaying a second message.

# Getting some Clojure

Finally, we just add a small MQTT library to our project.clj to get all set from the Clojure side:

```
[clojurewerkz/machine_head "1.0.0-beta9"]
```

Et voila! Now let's have some fun.

# How to do it...

In this receipe, we will see how to publish and subscribe to messages from Clojure. The machine_head library is a classic, and the following code will look very sensible to most of us:

```
(ns chapter04.mqtt-pub
  (:require [clojurewerkz.machine-head.client :as mh]))

(let [id (mh/generate-id)
      c  (mh/connect "tcp://127.0.0.1:1883" id)]
  (mh/publish c "topic/hello" "hello from Clojure!")
  (mh/disconnect c))
```

We start by requiring the library with the mh prefix, preparing an ID for