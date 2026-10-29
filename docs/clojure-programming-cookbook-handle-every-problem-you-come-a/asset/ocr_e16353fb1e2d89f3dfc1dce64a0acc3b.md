documentation around the Interweb, we will use the new library messaging framework Artemis, included in the latest releases of WildFly.

The project.clj file gets just a bit complicated, due to the fact we want to avoid including the hornetq library, but this should be fixed soon:

```
(defproject mutant2 "0.1.0-SNAPSHOT"
  :dependencies [
    [org.clojure/clojure "1.8.0"]
    [org.immutant/immutant "2.1.4"
      :exclusions [org.projectodd.wunderboss/wunderboss-messaging-hornetq]]
    [org.projectodd.wunderboss/wunderboss-artemis "0.2.0"]
    [ring/ring-devel "1.5.0"]
    [compojure "1.5.0"]])
```

You can also note the (re-)introduction of Compojure and you are ready to go!

The following snippet actually asks the framework to create a new messaging server, through the slightly misleading msg/context function:

```
(require '[immutant.web :as web] '[immutant.messaging :as msg])
(def messaging-server
  (msg/context :remote-type :hornetq-standalone))
```

Once the messaging server has been programmatically started, we can create a context that will be used to look up queues and topics.

Here, we connect to the messaging server just started, and look up the hello queue. This will actually create the queue if it does not exist, or simply connect to it if another service has already registered it:

```
(def mycontext
  (msg/context :host "localhost"))

(def queue
  (msg/queue "hello" :context mycontext))
```

Most of the new function calls are done using the immutant.messaging namespace, labelled as msg.