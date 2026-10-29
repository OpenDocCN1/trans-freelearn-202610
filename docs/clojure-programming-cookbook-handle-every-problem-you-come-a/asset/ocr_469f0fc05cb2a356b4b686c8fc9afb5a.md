```
// Comment to get more information during initialization
//logLevel := Level.Debug
logLevel := Level.Info
//lazy val root =
project.in(file(".")).dependsOn(clojurePlugin)

//lazy val clojurePlugin = file("../../sbt-clojure")
addSbtPlugin("com.unhandlederexpression" % "sbt-clojure" % "0.1")
```

# Writing source code

We will create the Clojure source code under src/main/clojure:

```
(ns helloworld)
(gen-class
  :name cljbook.chapter05.HelloWorld
  :methods [#^{:static true} [sayHello [] void]])
(defn -sayHello []
  (println "Hello world from Clojure!"))
(gen-class
  :name cljbook.chapter05.Calculator
  :methods
  [#^{:static false} [add [long long] long]
   #^{:static false} [sub [long long] long]])
(defn -add [this a b]
  (+ a b))
(defn -sub [this a b]
  (- a b))
```

The preceding Clojure code has two classes:
cljbook.chapter05.HelloWorld and cljbook.chapter05.Calculator.
Then, we will look at Scala code. The following is the Scala code for calling classes defined in Clojure:

```
import cljbook.chapter05.HelloWorld
import cljbook.chapter05.Calculator
object Main {
  def main(args: Array[String]) = {
    println("I'm calling methods defined by Clojure from Scala.")
    HelloWorld.sayHello();
    val calc = new Calculator
    println("add: "+calc.add(1,2))
    println("sub: "+calc.sub(10,5))
  }
```