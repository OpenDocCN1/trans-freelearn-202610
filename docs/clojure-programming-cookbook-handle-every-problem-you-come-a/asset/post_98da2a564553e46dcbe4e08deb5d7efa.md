
```
(def actor-1
  (okku/spawn
    (okku/actor
      (okku/onReceive [m]
        (println m)))
    :in actor-system :name "print-m"))
;;=> #'chapter06.akka/actor-1
```

The tell function sends a message to an actor:

```
(defn tell [actor message]
  (.tell actor message nil))
;;=> #'chapter06.akka/tell
```

Now, let's send a string message and make sure the actor receives a message:

```
(tell actor-1 "Hello Akka Actor !")
;;=> Hello Akka Actor !
;;=> nil
```

Then, we will send a map message:

```
(tell actor-1 {:name "Makoto" :country "Japan" :message "Hello !"})
;;=> {:name Makoto, :country Japan, :message Hello !}
;;=> nil
```

## Creating actors

okku/actor creates akka.actor.props, and okku/spawn creates Actor objects using props. The following code generates the props object:

```
(def print-props
  (okku/actor
    (okku/onReceive [m]
      (println m))))
;;=> #'chapter06.akka/print-props
```

okku/spawn creates actors using props as follows:

```
(def print-actor
  (okku/spawn
    print-props
    :in actor-system :name "print-actor3"))
```

```
;;=> #'chapter06.akka/print-actor

Then, send a message to `print-actor`:

(tell print-actor "hello !")
;;=> hello !
;;=> nil
```

## Dispatching messages

okku has the `dispatch-on` function to dispatch messages with message types. The following `print-actor-by-key-props`:

```
(def print-actor-by-key-props
  (okku/actor
    (okku/onReceive [{type :type message :message}]
      (okku/dispatch-on type
        :normal (println "message: " message)
        :emergency (println "emergency !!!! : " message)
        :else (println "unhandled type")))))
;;=> #'chapter06.akka/print-actor-by-key-props
(def print-actor-by-key
  (okku/spawn print-actor-by-key-props
              :in actor-system
              :name "print-actor-by-key")
)
;;=> #'chapter06.akka/print-actor-by-key
```

Let's send two types of messages!

```
(tell print-actor-by-key {:type :normal :message "I'm very happy !"})
;;=> message: I'm very happy !
;;=> nil
(tell print-actor-by-key {:type :emergency :message "I don't have money. Please give me some !"})
;;=> emergency !!!! :  I don't have money. Please give me some !
;;=> nil
```

The following is another example of dispatch on classes:

```
(def print-actor-by-type-props
  (okku/actor
    (okku/onReceive [message]
      (okku/dispatch-on (class message)

clojure.lang.PersistentVector (println "vector => "message)

clojure.lang.PersistentArrayMap (println "array-map =>
"message)
clojure.lang.PersistentHashMap (println "hash-map => "message)
:else (println
"unhandled type")
))))
;;=> #'chapter06.akka/print-actor-by-type-props
(def print-actor-by-type
  (okku/spawn print-actor-by-type-props
    :in actor-system
    :name "print-actor-by-type")
)
;;=> #'chapter06.akka/print-actor-by-type
```

## Let's test the preceding code:

```
(tell print-actor-by-type [1 2 3])
;;=> vector =>  [1 2 3]
;;=> nil
(tell print-actor-by-type {:a 1 :b 2})
;;=> array-map =>  {:a 1, :b 2}
;;=> nil
(tell print-actor-by-type {:v1 1 :v2 2 :v3 3 :v4 4 :v5 5 :v6 6
:v7 7 :v8 8 :v9 9})
;;=> hash-map =>  {:v1 1, :v2 2, :v3 3, :v4 4, :v5 5, :v6 6,
:v7 7, :v8 8, :v9 9}
;;=> nil
```

## How it works...

We will explain actor systems and actors in okku.

## Actor system

Actor systems are entry points for actors and manage actors. We can create actors from actor systems. We defined actor-system in the preceding section. Let's see the definition as follows:

```
actor-system
;;=> #object[akka.actor.ActorSystemImpl 0x59236b90
"akka://actor-system-1"]
(class actor-system)
;;=> akka.actor.ActorSystemImpl
```

Okku's actor system is akka.actor.ActorSystemImpl, which is a subclass of akka.actor.ActorSystem, as shown in the following image:

![](img/d7b631352a782ee50160cf8eb995152f_392_0.png)

Actor systems have their addresses. The address of our actor system is akka://actor-system-1.

## Understanding actors

Actors have internal states similar to objects in object-oriented systems. However, actors don't have public methods. Instead, actors communicate via mail boxes. Actors usually process messages asynchronously. By default, they process messages in the order of their arrival but it can process different orders using priorities.

Actors sometimes delegate their tasks to child actors. Parent actors generate children and they request subtasks to their child actors. This workflow is explained in the following illustration:

![](img/d7b631352a782ee50160cf8eb995152f_393_0.png)

Okku's actor `akka.actor.RepointableActorRef` is created by `okku/spawn__`:

```
print-actor
;;=> #object[akka.actor.RepointableActorRef 0x186dd109 "Actor[akka://actor-system-1/user/print-actor#-652953659]"]
(class print-actor)
;;=> akka.actor.RepointableActorRef
```

Actors are created under an actor system. In the preceding example, `print_actor` is created as follows:

```
akka://actor-system-1/user/print-actor.
```

## There's more...

We will learn an advanced usage for Okku in this recipe.

## Calling remote actors

Here, we will show you how to communicate with remote actors. We will create a new `akka` project named `akka-service` and will define `hello-actor` under `actor-system-1`:

```
$ lein new akka-service
```

Go to the akka-service_ directory and modify _project.clj as follows:

```
:dependencies [
    [org.clojure/clojure "1.8.0"]
    [org.clojure.gaverhae/okku "0.1.5"]
]
```

We have to add the main: section as follows:

```
:main akka-service.core
```

Modify src/akka-service/core.clj as follows and save it:

```
(ns akka-service.core
  (:require [okku.core :as okku]))
(def actor-system (okku/actor-system "actor-system-1"
                    :hostname "localhost"
                    :port 2552))
(defn -main [& args]
  (println "launching akka service ...")
  (okku/spawn
    (okku/actor
      (okku/onReceive [m]
        (println m)))
    :in actor-system :name "hello-actor")
  )
```

The following command runs actor system and actor on localhost, and the port number is 2552:

```
$ lein run
```

The following image shows the output of the preceding command:

![](img/d7b631352a782ee50160cf8eb995152f_395_0.png)

Now, we will access the preceding actor system and actor from the different REPL. So, let's start another REPL as follows:

```
```clojure
$ lein repl
```
```

Then, define namespace and load `okku` library. After that, create an actor system named `actor-system-2` and get the reference of the remote actor:

```
```clojure
(ns okku-client.core
  (:require [okku.core :as okku]))
;;=> nil
(defn tell [actor message]
  (.tell actor message nil))
;;=> #'okku-client.core/tell
(def actor-system
  (okku/actor-system "actor-system-2" :hostname "localhost"
  :port 2553))
;;=> #'okku-client.core/actor-system
(def actor-ref (okku/lookup
                "akka.tcp://actor-system-1@localhost:2552/user/hello-
actor"
                :in actor-system))
;;=> #'okku-client.core/actor-ref
```
```

The name of the remote actor system is `actor-system-1` and its port is

2552. The path to the target actor is /user/hello-actor/.

Let's invoke the remote actor via the reference:

```
(tell actor-ref "hello remote akka !")
;;=> nil
```

We can see the message in another REPL:

![](img/d7b631352a782ee50160cf8eb995152f_396_0.png)

## Request and reply

We will implement request/reply messages between actors. Since communications between actors are asynchronous, receivers should reply to messages explicitly using okku.core/!

Let's restart the actor system:

```
(.shutdown actor-system)
;;=> nil
(def actor-system (okku/actor-system "actor-system-1" :hostname "localhost"))
;;=> #'chapter06.akka/actor-system
```

We will define stock-service-requester and stock-service-requester-actor to request the current stock price of the company. declare for stock-service-actor is necessary because it will be defined after the definition of stock-service-requester:

```
(declare stock-service-actor)
;;=> #'chapter06.akka/stock-service-actor
(def stock-service-requester
  (okku/actor
```
