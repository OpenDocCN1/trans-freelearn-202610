
```
; put some Celsius values in the original channel
(>!! in-chan 30)
; 86

; remember that >!! runs on the main thread, and so if you want to
; make the code async as well, you can use go and >! As above.
(go (>! in-chan 10))
```

## Pub/sub

While we are looking at known stuff, a less famous pattern of core async is the included, easy-to-use pub/sub mechanism.

Let's have a look at some sushis and burgers. Create a publisher from a channel using pub, the channel, and a message key:

```
; publish
(def input-chan (chan))
(def our-pub (pub input-chan :food))
```

You can then define subscribers to this publisher using sub, the pub just defined, and an output channel:

```
; subscribe
(def burger-chan (chan))
(sub our-pub :burger burger-chan)

(def sushi-chan (chan))
(sub our-pub :sushi sushi-chan)
```

Finally, with some glue code to print the values from subscribers from different channels:

```
(defn print-loop [channel prefix]
  (go-loop []
    (let [{:keys [option]} (<! channel)]
      (println prefix " " option)
      (recur))))
(print-loop sushi-chan :sushi)
(print-loop burger-chan :burger)
```

You can know dispatch food at will and have a great lunch:

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

## How to do it...

Let's get back to the simple Celsius to Fahrenheit example and see how we can enhance things a bit using regular sockets, combined with core async channels. Remember you included the com.gearswithingears.async-sockets library in the project; we will make use of that just now.

### Async socket server

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

```
(async/go-loop []
    (when-let [line (async/<! (:in socket))]
(async/>! (:out socket)
(str "FAHRENHEIT: " (c-to-f line)))
(recur)))
```

Finally, the main server loop is just accepting connections and adding them to a regular core async channel:

```
(let [server (socket-server 12345)]
    (async/go-loop []
        (when-let [connection (async/<! (:connections server))]
            (handle-connection connection)
            (recur))))
```

![](img/d7b631352a782ee50160cf8eb995152f_433_0.png)

A bit of Telnet to talk to the socket and converts values. Note that for the purpose of this book, the socket server is actually running on a simple Raspberry Pi.

### Async socket client

The example would not be complete if there was not a client version of this. Since all this code is asynchronous, you can use the following code in the same REPL you were using until now. This is also using an async channel to put values on the socket connection and reads quite nicely:

```
```clojure
(def socket (socket-client 12345 "localhost")
(async/>!! (:out socket) "10")
(println (async/<! (:in socket)))
; CELCIUS TO FAHRENHEIT: 50.0

; close the socket.
(close-socket-client socket)
```
```

### Chiming

Sockets are nice, but a simple and fun way of showing that async's work is to deal with sound. Chime is a simple timer library that provides a channel around timed events. So, to play a short audio clip after a few seconds, you can pass init values to chime and it will push messages on the async channel, always on time, just like a Japanese train.

This example will do just that by asking chime to send a message to a core async channel after two seconds. The handling block will then, without blocking, play the sample audio clip in the background.

Let's start with the usual namespaces declarations:

```
```clojure
(ns asyncing.withchime
  (:import [javax.sound.sampled AudioSystem])
  (:require [chime :refer [chime-ch]]
            [clj-time.core :as t]
            [clojure.java.io :as io]
            [clojure.core.async :as a :refer [<! go-loop]]))
```
```

This is nothing more than a simple Java interop code block that will play a sound from a wav file:

```
```clojure
(defn play-sound [file]
  (let[
    sound (io/as-file file)
    input (AudioSystem/getAudioInputStream sound)
    clip (AudioSystem/getClip)]
    (doto clip
      (.open input)
      (.start))))
```
```

Finally, the chime channel takes a vector of clj-time inputs, here two

seconds from now, and creates what can be seen as a simple cron channel:

```
(let [chimes (chime-ch [ (-> 2 t/seconds t/from-now) ])]
  (a/<!! (go-loop []
    (when-let [msg (<!! chimes)]
      (play-sound "resources/pendolino.wav")
      (recur)))))
```

At the REPL, and after two seconds, you can hear the sound of a bullet train passing by and you can still keep on typing code.

## There's more...

To close this recipe, we will cover more on sockets, this time using WebSockets, with a bit of a mix with Python code.

### Client to Python WebSockets

With pip, the Python library manager installed, we will install the tornado server that has WebSocket support:

```
sudo pip install tornado
```

Then a simple WebSocket server can be written as follows. Since we do not want to go deep into Python details, it's best to simply copy and paste the following in a file named websockets.py for now:

```
import tornado.httpserver
import tornado.websocket
import tornado.ioloop
import tornado.web
import socket
```

```
class WSHandler(tornado.websocket.WebSocketHandler):
    def open(self):
        print 'new connection'

    def on_message(self, message):
        print 'sending back message: %s' % message
        self.write_message(message)

    def on_close(self):
```

```
print 'connection closed'

def check_origin(self, origin):
    return True

application = tornado.web.Application([
    (r'/ws', WSHandler),
])

if __name__ == "__main__":
    http_server = tornado.httpserver.HTTPServer(application)
    http_server.listen(8888)
    myIP = socket.gethostbyname(socket.gethostname())
    print '*** Websocket Server Started at %s***' % myIP
    tornado.ioloop.IOLoop.instance().start()
```

Basically, we implemented a WebSocket handler at path /ws and we hook this into a simple Tornado application. Running the server is then simply done through:

```
python websockets.py
```

Now that we have this, we can write some WebSocket client code from Clojure using the jetty-websocket-async library:

```
(require '[com.keminglabs.jetty7-websockets-async.core :refer [connect!]]
        '[clojure.core.async :refer [chan go >! <! >!! <!!]])

(def c (chan))
(connect! c "ws://localhost:8888/ws")
```

The core async chan contains the connection and we can then retrieve it to send messages to the Python WebSocket server with:

```
(go
  (loop []
    (when-let [ws-req (<! c)]
      (>! (:in ws-req) "Hello remote websocket server!")
      (recur))))
```

To make it more obvious that we can reuse those WebSockets connections, we confirm that we have two channels, in and out, to the

### WebSocket server:

```
(def in (atom nil))
(def out (atom nil))
```

Here, notice that both in and out are core async channel, as prepared for us by the Jetty WebSocket library:

```
(go
  (let [ws-req (<! c)]
    (reset! in (:in ws-req))
    (reset! out (:out ws-req))))
```

```
(defn printer
  [in]
  (go (while true (println (<! in)))))
```

```
(printer @out)
(>!! @in "sushi please")
```

### Clojure WebSocket server

This has been covered in other places, so we will quickly glance at this Clojure WebSocket server. You can already see that contrary to the Python code, this feels naturally integrated into our async programming code:

```
(require '[com.keminglabs.jetty7-websockets-async.core :refer [configurator]]
        '[clojure.core.async :refer [chan go >! <!]]
        '[ring.adapter.jetty :refer [run-jetty]])
```

```
(defn http-handler
  [req]
  {:response 200 :body "HTTP hello" :headers {}})
```

```
; this is the channel where new connections will be put in
(def c (chan))
```

```
(def ws-configurator (configurator c {:path "/ws"}))
```

```
(def server
  (run-jetty http-handler {:configurator ws-configurator
                            :port 8888, :join? false}))
```
