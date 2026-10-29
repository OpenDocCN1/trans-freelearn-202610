# Using third-party libraries

So, you have found someone else's code that you would like to use, and you are trying to take their work and use it in your project, great! This is what this recipe is all about. There are a few ways to get the code closer to you.

# Getting ready

This recipe will introduce you to the art of adding dependencies, packaged as JAR files, to your project and how to reference them, as well as using them from your Clojure code. We will go from downloading the file and starting Clojure REPL manually, to using dependency management tools. Lastly, we will also present how to add new dependencies at runtime, so you do not need to restart your live programming environment.

# How to do it...

Each minor section in this recipe shows you how to add the dependency, using different ways in different scenarios.

# Adding the JAR file manually to your classpath

Say we have found the following library, clj-tuples, and we want to add this to our REPL session. Most JARs for Clojure are available on either mvnrepository.com or clojars.org.

clj-tuples is on clojars, and since clojars is a regular Maven repository, we can navigate directly through the file. Download and save it (https://clojars.org/repo/ruiyun/tools.timer/1.0.1/tools.timer-1.0.1.jar), and now let's start a Clojure REPL:

```
java -cp .:clj-tuple-0.2.2.jar:clojure-1.8.0-beta1.jar
clojure.main
```

And let's quickly have fun with our new library code...mmmmm, tuples: