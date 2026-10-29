# How it works...

The preceding code uses file endpoints. We will show you major parameters for the endpoint as follows:

| Name | Default value | Description |
|---|---|---|
| noop | false | If true, the file is not moved or deleted in any way. This option is good for read only data or for ETL type requirements. |
| append | true | When writing, do we append to the end of the file or replace it? |
| consumer.delay | 500 | Milliseconds before the next poll of the file/directory. |

# There's more...

Let's define a very simple processor that transforms messages between the file endpoints. To write a processor, we have to implement a process method in the org.apache.camel Processor class. We will use the Clojure proxy function to implement this method. Our simple processor echoes an inbound string and adds a string "Hello " on it and sends it to the outbound:

```
(defn get-payload-as-string [ex]
  (-> ex .getIn (.getBody (class ""))))
;;=> #'chapter04.camel/get-payload-as-string
(def simple-processor
  (proxy [Processor] []
    (process [exchange]
      (let [payload
            (get-payload-as-string exchange)]
        (println payload)
        (-> exchange .getOut (.setBody (str "Hello " payload)))))))
#'chapter04.camel/processor
```

So, let's define a new context using the processor:

```
(defn file-to-file-with-processor []
```