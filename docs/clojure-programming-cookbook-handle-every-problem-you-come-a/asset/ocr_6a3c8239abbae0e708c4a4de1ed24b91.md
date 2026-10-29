```
(defproject quickchecks "0.1.0-SNAPSHOT"
  :profiles {:dev {:dependencies [
  [midje "1.8.3"]

  :plugins [[lein-midje "3.2"]]}}}
  :dependencies [
  [org.clojure/clojure "1.8.0"]
  [compojure "1.5.1"]

  [org.clojure/test.check "0.9.0"]
  [com.velisco/herbert "0.7.0"]
  [com.gfredericks/test.chuck "0.2.6"]
  ])
```

Let's put all this into practice!

# Generating random values or groups of values

To understand property-based testing, we will start by having a look at some methods to generate random inputs.

Random inputs are generated using...generators. No, seriously. This is the name the test.check library is using.

You can see the output of a generator using another simple generator, named sample. sample takes a generator, and by default generates 10 values taken from the input generator.

In the following first example, the sample generator takes a generator that always returns the same value, 1.

Let's start an REPL:

```
(require '[clojure.test.check.generators :as gen])

(gen/sample (gen/return 1))
; (1 1 1 1 1 1 1 1 1)
```

As expected, sample returns 10 times the same value, here 1. The default size for the sample is 10, but you can also set a given size:

```
(gen/sample (gen/return 1) 5)
```