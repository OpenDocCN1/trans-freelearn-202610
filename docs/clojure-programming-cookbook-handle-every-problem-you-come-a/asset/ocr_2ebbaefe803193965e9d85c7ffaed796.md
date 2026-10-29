```
(def reactor
  (mr/create))

; function to proxy messages from mqtt to
; the above created reactive point
(defn proxy-to-meltdown
  [^String topic _ ^bytes payload]
  (mr/notify reactor topic (String. payload)))

; subcribe to mqtt messages and proxy
; all message to the function defined
(mh/subscribe
  conn
  ["#"]
  proxy-to-meltdown)

; lastly, when the reactor receives any message
; print the event
(mr/on reactor
  (sel/match-all)
  (fn [event]
    (println (:data event))))
```