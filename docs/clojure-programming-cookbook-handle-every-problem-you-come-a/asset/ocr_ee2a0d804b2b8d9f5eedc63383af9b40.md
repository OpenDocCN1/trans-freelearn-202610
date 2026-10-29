```
(>!! input-chan {:food :burger :option "with cheese"})
(>!! input-chan {:food :burger :option "with avocado"})
```

Also remember that thread can be used to spawn a separate thread and run its own inline code block:

```
(thread
  (>!! input-chan {:food :sushi :option "salmon"}))
```

Now that we have all this async code in mind, let's see how to integrate it nicely with other tools.

# How to do it...

Let's get back to the simple Celsius to Fahrenheit example and see how we can enhance things a bit using regular sockets, combined with core async channels. Remember you included the com.gearswithingears.async-sockets library in the project; we will make use of that just now.

# Async socket server

The idea here is to delegate the handling of connections to a channel, which itself will handle values coming to it through another channel.

Let's start with the glue code to include the library in the namespace, and the Celsius to Fahrenheit conversion code:

```
(ns asyncing.withsockets
  (:require [com.gearswithingears.async-sockets :refer :all]
            [clojure.core.async :as async]))
```

```
(defn c-to-f [c]
  (+ (* 1.8 (Integer/parseInt c)) 32))
```

Then, let's go to the connection handling block. On the socket connection, this function creates a new async loop that repeatedly reads lines of input. Each input is supposed to be an integer and the value is then simply converted to Fahrenheit and returned to the client:

```
(defn handle-connection [socket]
```