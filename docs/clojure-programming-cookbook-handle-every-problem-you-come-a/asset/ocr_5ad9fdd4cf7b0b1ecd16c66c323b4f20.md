either create a new Clojure namespace or modify your existing one so that it looks like the following (the handler function must be named - handleRequest since handleRequest is specified as part of the interface):

```
(ns aws-examples.lambda-example
  (:gen-class
    :implements
    [com.amazonaws.services.lambda.runtime.RequestStreamHandler])
  (:require [clojure.java.io :as io]
            [clojure.data.json :as json]
            [clojure.string :as str]))
(defn -handleRequest [this is os context]
  (let [w (io/writer os)
        parameters (json/read (io/reader is) :key-fn keyword)]
    (println "Lambda Hello Stream Output ")
    (println "this class: " (class this))
    (println "is class:" (class is))
    (println "os class:" (class os))
    (println "context class:" (class context))
    (println "Parameters are " parameters))
    (.flush w)))
```

Use lein uberjar again to create a JAR file. Since we have an existing Lambda function in AWS, we can overwrite the JAR used in the Hello World example. Since the handler function name has changed, we must modify our Lambda configuration to match. This time, the default test that provides parameters in JSON format should work as-is.

We can very easily get a more interesting test of Hello Stream by configuring this Lambda to run whenever a file is uploaded to S3. At the Lambda management page, choose the Event Sources tab, click on Add Event, and choose an S3 bucket to which you can easily add a file. Now, upload a file to the specified S3 bucket and then navigate to the logs of the Hello World Lambda function. You will find that Hello World has been automatically invoked, and a fairly complicated object that represents the uploaded file is supplied as a parameter to our Lambda function.

# Real-world Lambdas

To graduate from a Hello World Lambda to real-world Lambdas, the