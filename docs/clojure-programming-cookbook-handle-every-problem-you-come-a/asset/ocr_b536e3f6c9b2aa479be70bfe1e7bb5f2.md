```
(:use [clojure.java.shell :only [sh]])
(:require [morse.polling :as p])
(:require [morse.handlers :refer :all])
...)

(def token "<your bot token here ...>")

(def last-value
  (atom ""))

; telegram bot handler: update the atom when a message is received
(defhandler bot-api
  (message
    message
    (dosync (reset! last-value (:text message)))))

; for this example, simply start a polling method to get events
(def channel
  (p/start token bot-api))
...
```

From there, messages sent to the bot will be picked up by the Raspberry Pi and shown on browsers connecting to the Vaadin application: