source file uses the Clojure namespace helloworld. The entry point to your Lambda code is a Clojure function that is exposed as a method of a Java class using Clojure's gen-class.

Similar to use and require, the gen-class function can be included in the Clojure ns definition, as in the following, or specified separately. You can use any name you want for the handler function but the prefix must be a hyphen unless an alternate prefix is specified as part of the :methods definition:

```
(ns aws-example.lambda-helloworld
  (:gen-class
    :methods [^:static [handler [String] String]]))
(defn -myhandler [s]
  (println (str "Hello," s)))
```

From the command line, use lein uberjar to create a JAR that can be uploaded to AWS Lambda.

# Hello World - the AWS part

Getting your Hello World to work is now a matter of creating a new Lambda within AWS, uploading your JAR, and configuring your handler.

# Hello Stream

The handler method we used in our Hello World Lambda function was coded directly and could be extended to accept custom Java classes as part of the method signature. However, for more complex Java integrations, implementing one of AWS's standard interfaces for Lambda is both straightforward and feels more like idiomatic Clojure. The following example replaces our own definition of a handler method with an implementation of a standard interface that is provided as part of the aws-lambda-java-core library. First of all, add the dependency [com.amazonaws/aws-lambda-java-core "1.0.0"] into your project.clj. While you are modifying your project.clj, also add in the dependency for [org.clojure/data.json "0.2.6"] since we will be manipulating JSON-formatted objects as part of this exercise. Then,