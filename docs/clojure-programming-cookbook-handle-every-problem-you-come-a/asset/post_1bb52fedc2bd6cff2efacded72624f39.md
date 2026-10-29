
To call Java methods, the syntax is as follows:

```
( .method-name object arg1 arg2 ... )
```

The following code calls the Java concat method:

```
( .concat "Hello " "Makoto")
;;=> "Hello Makoto"
```

## Calling class methods

The syntax for calling static methods (that is, class methods) is as follows:

```
(class-name/method-name arg1 arg2 ...)
```

Or:

```
( . class-name method-name arg1 arg2 ... )
```

The following code is to get a float value from a string:

```
(Float/valueOf "1000.0")
;;=> 1000.0
( . Float valueOf "1000.0")
;;=> 1000.0
```

Another example is to get the Japanese timezone using the static method:

```
(import 'java.util.TimeZone)
(def time-zone (TimeZone/getTimeZone "Asia/Tokyo" ))
;;=> #'chapter05.calling-java/time-zone
(.getDisplayName time-zone (java.util.Locale. "en_US"))
;;=> "Japan Standard Time"
```

## Accessing instance fields

Direct access to instance fields is not recommended in Java, so it's not often we access instance fields. However, there is a way to access these fields. The syntax is as follows:

```
( . field-name object)
```

We will access instance fields of java.awt.Point as follows:

```
(import java.awt.Point)
;;=> java.awt.Point
(def p (Point. 100 200))
;;=> #'chapter05.calling-java/p
(. p x)
;;=> 100
(. p y)
;;=> 200
```

The syntax for setting instance values is as follows:

```
(set! (.field-name object) value)
```

The following code is to update the instance field of java.awt.Point:

```
(set! (.x p) 300)
;;=> 300
(. p x)
;;=> 300
```

## Accessing class fields

The syntax for accessing static fields is as follows:

```
class-name/field-name
```

The following code gets the value of the natural logarithm:

```
Math/E
;;=> 2.718281828459045
```

## Accessing inner classes

The following syntax is to access static inner classes:

```
class-name$inner-class-name/static-field-name
```

Or:

```
(class-name$inner-class-name/static-method-name arg1 arg2 ..)
```

The following example accesses the static values NEW and TERMINATED in
an inner class of the Thread class:

```
(def t (Thread.))
;;=> #'chapter05.calling-java/t
(.getState t)
;;=> #object[java.lang.Thread$State 0xc736e3e "NEW"]
(= (.getState t) Thread$State/NEW)
;;=> true
(.start t)
;;=> nil
(= (.getState t) Thread$State/TERMINATED)
```

The next example shows how to access methods in an inner class. At the time of creating a thread, its state is NEW. After the thread is started, it terminates immediately and the state becomes TERMINATED:

```
(Thread$State/values)
;;=> #object["[Ljava.lang.Thread$State;" 0x3efd737c "
[Ljava.lang.Thread$State;@3efd737c"]
(Thread$State/valueOf "NEW")
;;=> #object[java.lang.Thread$State 0xc736e3e "NEW"]
```

## Referencing classes

To refer to classes, use their class name as follows:

```
String
;;=> java.lang.String
(= String (Class/forName "java.lang.String"))
;;=> true
(= java.lang.String (Class/forName "java.lang.String"))
;;=> true
```

## Using arrays

To create arrays in Java, use into-array:

```
(def str-array (into-array '("a" "b" "c" "d" "e")))
;;=> #'chapter05.calling-java/str-array
```

To access arrays, use aget:

```
(aget str-array 0)
;;=> "a"
```

Arrays in Java are not immutable, so we can change the value of the element using aset:

```
(aset str-array 0 "x")
;;=> "x"
(aget str-array 0)
;;=> "x"
```

long-array creates arrays whose elements are the long type of Java primitives. We will compare the performance with an array whose elements are Java's long objects:

```
(time
  (let [n 10000000
        a (long-array (range n))]
    (reduce + a)))
;;=> "Elapsed time: 1339.504072 msecs"
;;=> 49999995000000
(time
  (let [n 10000000
        a (into-array (range n))]
    (reduce + a)))
;;=> "Elapsed time: 6101.370939 msecs"
;;=> 49999995000000
```

The following code creates Java's byte-array:

```
(byte-array (map int [\C \l \o \j \o \u \r \e]))
;;=> #object["[B" 0x590f9b92 "[B@590f9b92"]
(String. (byte-array (map int [\C \l \o \j \o \u \r \e])))
;;=> "Clojoure"
```

The following code is to create a two-dimensional array:

```
(def mat1 (make-array Double/TYPE 10 10))
;;=> #'chapter05.calling-java/mat1
(aget mat1 9 9)
;;=> 0.0
```

To specify, Double/TYPE generates an array whose elements are double primitive types.

## How it works...

We will summarize ways to interoperate Java and compare its syntax with Java calls. Then we will explain how Clojure does not inherit Java's checked exceptions.

## Summary of how Clojure accesses Java methods and objects

In the following table, we summarize how Clojure can invoke Java methods and access Java objects:

| Action | Syntax | Clojure code | Java code |
|---|---|---|---|
| Creating instances | (ClassName. arg1 arg2 ...) | (String. "abc") | new String("abc"); |
| Calling instance methods | (.method-name object arg1 arg2 ...) | (.concat "Hello " "Makoto") | String s = "Hello "; s.concat("Makoto"); |
| Calling class methods | (class-name/method-name arg1 arg2 ...) | (Float/valueOf "1000.0") | Float.valueOf("1000.0"); |
| Accessing instance fields | (. field-name object) | (. (java.awt.Point. 100 200) x) | Point new java.awt.Point(100 200); p.x; |
| Accessing class fields | class-name/field-name | Math/E | Math.E; |
| Accessing fields of inner classes | class-name$inner-class-name/static-field-name | Thread$State/NEW | Thread.State.NEW; |
| Calling methods of inner classes | (class-name$inner-class-name/static-field-name arg1 arg2 ..) | (Thread$State/values) | Thread.State.values(); |
| Referencing classes | class-name | java.lang.String | Class.forName("java.lang.String"); |

## Clojure does not inherit Java's checked exception

In Java, checked exceptions are checked at compile time. Using Java, checked exceptions should be handled by a try-catch block or methods should declare the exception using the throws keyword. Otherwise the program will give an error at compile time.

The following Java code causes an error at compile time:

```
class CheckedException
{
    public static void main(String args[])
    {
    Thread.sleep(1000);
    }
}
```

```
$ javac CheckedException.java
CheckedException.java:3: error: unreported exception
InterruptedException; must be caught or declared to be thrown
    Thread.sleep(10000);
                ^
1 error
```

To avoid the preceding error, we need to modify the code as follows:

```
public class CheckedException {
    public static void main(String args[])  {
    try {
        Thread.sleep(10000);
    } catch (InterruptedException e) {
        System.out.println("Interrupt occured");
    }
    }
}
```

Generally, checked exceptions tend to not be liked. Scala also does not inherit checked exceptions. Clojure is the same, and the following code works fine:

```
(Thread/sleep 1000)
```

## Summary of array accesses

In the following table, we summarize Java's array accesses from Clojure and equivalent code using Java:

| Action | Clojure | Java |
|---|---|---|
| Creating a one-dimensional array | (long-array 10) | new long[10]; |
| Creating a two-dimensional array | (make-array Double/TYPE 10 10) | new Double[10][10]; |
| Getting an element of an array | (aget str_array 0) | str_array[0]; |
| Setting a value to an element of an array | (aset str_array 0 "x") | str_array[4] = "x"; |

## Print strings of Java's primitives

We often see unusual expressions such as the following when we generate an array whose element type is primitive:

```
(int-array [1 2])
;;=> #object["[I" 0x4d307a58 "[I@4d307a58"]
```

In the preceding example, "[I" stands for an array of int. Similarly, the next example generates the two-dimensional array with Double/TYPE:

```
(make-array Double/TYPE 10 10)
;;=> #object["[[D" 0x77570b3c "[[D@77570b3c"]
```

In the preceding example, "[[D" stands for two-dimensional array of type double. The following table shows how Java primitives are printed:

| Type | String |
|---|---|
| Short | "S" |
| Integer | "I" |
| Long | "J" |
| Float | "F" |
| Double | "D" |
| Byte | "B" |
| Char | "C" |
| Boolean | "Z" |

## There's more...

We will learn how to use more advanced topics in this section.

## Chained calls with the double-dot macro

The .. macro expands into a member access (.) of the first member on the first argument, followed by the next member on the result. Using (..) chains calls from Java methods. The code to get JAVA_HOME from REPL is as follows:

```
(.. System getenv (get "JAVA_HOME"))
;;=> "/usr/local/oss/jdk1.8.0_65"
```

The preceding is equivalent to:

```
(. (. System getenv) (get "JAVA_HOME")).
(macroexpand-1 '(.. System getenv (get "JAVA_HOME")))
;;=> (..(.. (. System getenv) (get "JAVA_HOME")))
```

## Using the doto macro

The code to apply multiple methods on the same object using Java's interop is as follows:

```
(doto (java.util.HashMap.)
  (.put "key1" "value1")
  (.put "key2" "value2")
  (.put "key3" "value3")
)
;;=> {"key1" "value1", "key2" "value2", "key3" "value3"}
```
