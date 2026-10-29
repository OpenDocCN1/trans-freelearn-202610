# Benchmarking with Criterium, performance tips, and other tools

In this recipe, we will learn how to test, measure, and improve the performance of your application. We will show you a Clojure-based benchmark tool.

# Getting ready

To use Criterium, we need to add the `criterium` library in your `project.clj` as follows:

```
(defproject performance-example "0.1.0-SNAPSHOT"
  :description "FIXME: write description"
  :url "http://example.com/FIXME"
  :license {:name "Eclipse Public License"
            :url "http://www.eclipse.org/legal/epl-v10.html"}
  :dependencies [[org.clojure/clojure "1.8.0"]
                 [criterium "0.4.4"]])
```

Then, restart your REPL.

# How to do it...

Here, we will show you how to test the performance of your code using Criterium.

# Using Criterium

Criterium is a micro benchmark tool for measuring the computation time of Clojure expressions.

It performs given expressions multiple times and reports statistical information including means and std-deviations of execution time. Criterium also considers JVM characteristics such as the optimization of JIT compiler and effects of GC.

Let's assume we will measure performances for `apply`, `reduce`, and