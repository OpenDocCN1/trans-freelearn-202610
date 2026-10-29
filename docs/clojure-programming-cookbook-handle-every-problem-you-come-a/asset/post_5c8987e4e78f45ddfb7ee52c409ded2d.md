
organization if the data handled is straightforward and simple.

Possibly the most interesting motivation to separate namespaces is the public API method.

## There's more...

In the previous section, we saw that having a namespace for the public API of your library is important, so the following will describe, with the potemkin library as an example, how to extract just what you need from your code to present the API you want to present:

```
tools.namespace, potemkin
```

## Namespace for public API

potemkin has a very interesting method, named import-vars, that allows something like a copy/paste of a function from a different namespace to a current one. If we remember the previous recipe, we can simply try the potemkin dependency as follows:

```
lein try potemkin "0.4.1"
```

Then we can select on-demand functions and clean up our public-facing namespace:

```
user=> (require 'potemkin)
(potemkin/import-vars [clojure.string reverse])
(user/reverse "hello")
```

This makes selecting functions for our namespace clean, without any code, mostly documentation.

This also makes a case for versioning our namespace, in case some breaking changes are introduced but there is no need to make a completely new version of your code.

## tools.namespace

Now, we saw just a few moments ago that we can force :reload on a namespace, but things get complicated when namespaces depend on each other, and you are not sure which one was reloaded and which one was not. This is where tools.namespace makes it easier for you to track all this for you. Let us start a REPL and try this out:

```
lein try org.clojure/tools.namespace "0.2.11"
```

As per the doc, the refresh function will scan all the directories on the classpath for Clojure source files, read their ns declarations, build a graph of their dependencies, and load them in dependency order. (You can change the directories it scans with set-refresh-dirs.)

So, at the REPL, the function we mostly need from tools.namespace is refresh:

```
(require '[clojure.tools.namespace.repl :refer [refresh]])
```

Now, supposing our file helloagain.clj is in the src folder so, src/helloagain.clj, and lein can find and load the file with require:

```
(require '[helloagain :refer :all])
user=> (only-one)
only one
```

We will quickly add a new function to our file:

```
(defn only-two []
  (println "only two"))
```

And call refresh to make sure we have the latest code:

```
user=> (refresh)
; :reloading (helloagain)
user> (helloagain/only-two)
; only two
```

And that is all. We have pulled the latest from our namespace code, and the required namespaces will be reloaded as needed.

## What's next?

In this chapter, we have seen how to create namespaces and how to inspect them. We also looked at how to organize those namespaces, and how to make sure we always have the code we want at the place we want. We remembered that a public API is a key point for creating namespaces as well as organizing code into functional modules.

The next step is to look at the workflow for reloading a full application that has dependent components. Those components will require a workflow to manage their state and the now famous component library has been established as the way to go.

Maybe this could also be part of a future Clojure version, in which the need to break namespaces into something smaller than files will be handled.

## Chapter 2. Interacting with Collections

In this chapter, we will cover the following topics:

- Clojure collections and their basic functions
- Accessing and updating elements from collections
- Filtering elements from collections
- Transforming and sorting collections
- Merging and splitting collections
- How to become lazy

### Introduction

Clojure belongs to the Lisp language family, and Lisp can handle collections in a very cool and efficient way. Clojure inherits rich and cool sets from Lisp. In this chapter, we will show basic and advanced recipes for manipulating collections.

Another advantage of manipulating collections in Clojure is to guarantee immutability. In the case of large or concurrent applications, mutability causes unintended behaviors resulting in serious bugs.

After reading this chapter, you'll be able to write Clojure code to handle collections with a lot of fun.

## Clojure collections and their basic functions

There are four collection types in Clojure:

- Lists
- Vectors
- Maps
- Sets

In this recipe, we will describe what these types are and some basic functions for them.

### Getting ready

You only need REPL described in the recipe *Repl up!* in Chapter 1, *Live Programming with Clojure*, and no additional libraries. Start REPL so that you can review the sample code in this recipe.

### How to do it...

We will learn collection types in Clojure including lists, vectors, maps, and sets. We will learn how to create them and use basic functions for them.

### Lists

Lists are commonly used in Lisp. Clojure also supports the list data type. Lists are internally implemented as a linked list. To create a list, begin with quote (') and then enclose elements with (). If you want to create an empty list, use ' (), or (list):

```
'("A Study in Scarlet"
"The Sign of the Four"
"The Hound of the Baskervilles"
"The Valley of Fear")
;;=> ("A Study in Scarlet" "The Sign of the Four" "The Hound of the Baskervilles" "The Valley of Fear")
```

```
' ()
;;=> ()
(list)
;;=> ()
```

There are three basic functions, car, cdr, and cons, to manipulate lists in traditional Lisp. Instead of them, Clojure uses first, rest, and conj. Let's see how they work:

- first takes the head element from a list:
  
  (first '(1 2 3 4 5))
  ;;=> 1
  (first '())
  ;;=> nil
  (first nil)
  ;;=> nil

- The first of an empty list or nil returns nil.

- rest returns a list after the first element:
  
  (rest '(1 2 3 4 5))
  ;;=> (2 3 4 5)
  (rest '())
  ;;=> ()
  (rest nil)
  ;;=> ()

- The rest of an empty list or nil returns an empty list.

- conj adds elements at the beginning:
  
  (conj '(2 3 4 5) 1)
  ;;=> (1 2 3 4 5)

- conj is a variadic function and can take multiple elements as its arguments:
  
  ```clj
  (conj '("d" "e" "f") "c" "b" "a")
  ;;=> ("a" "b" "c" "d" "e" "f")
  ```

### Vectors

A vector also represents a sequence of elements, like list, but it supports efficient access to its elements by index.

To create a vector, enclose its elements with []. To create an empty vector, use [] or (vector):

```
["A Study in Scarlet"
"The Sign of the Four"
"The Hound of the Baskervilles"
"The Valley of Fear"]
;;=> ["A Study in Scarlet" "The Sign of the Four" "The Hound of the Baskervilles" "The Valley of Fear"]
[]
;;=> []
(vector)
;;=> []
```

We can use first, rest, and conj with vectors. Let's begin with first and rest:

```
(first [1 2 3 4 5])
;;=> 1
(first [])
;;=> nil
(rest [1 2 3 4 5])
;;=> (2 3 4 5)
```

The rest of the vector looks like a returning list, but it is not list. It is ChunkedSeq:

```
(class (rest [1 2 3 4 5]))
;;=> clojure.lang.PersistentVector$ChunkedSeq
```

To add elements to a vector, use the conj function. conj appends elements to the vector:

```
(conj [1 2 3 4] 5)
;;=> [1 2 3 4 5]
```

### Maps

Maps are associative lists, and they can be accessed efficiently with keys and return their values.

To create a map, enclose key-value pairs with {}:

```
{:name "Arthur Ignatius Conan Doyle"
:born "22-May-1859"
:died "7-July-1930"
:occupation ["novelist" "short story writer" "poet" "physician"]
:nationality "scotish"
:citizenship "United Kingdom"
:genre ["Detective fiction", "fantasy", "science fiction", "historical novels", "non-fiction"]
:notable-works ["Stories of Sherlock Holmes" "The Lost World"]
:spouse ["Louisa Hawkins" "Jean Leckie"]
:no-of-children 5
}
;;=> {:genre ["Detective fiction" "fantasy" "science fiction" "historical novels" "non-fiction"], :occupation ["novelist" "short story writer" "poet" "physician"], :name "Arthur Ignatius Conan Doyle", :no-of-children 5, :nationality "scotish", :died "7-July-1930", :spouse ["Louisa Hawkins" "Jean Leckie"], :notable-works ["Stories of Sherlock Holmes" "The Lost World"], :citizenship "United Kingdom", :born "22-May-1859"}
```

To create an empty map, use {} or (hash-map):

```
{}
;;=> {}
(hash-map)
;;=> {}
```

We can use first, rest, and conj for maps, just as for lists or vectors:

```
(first {:a 1, :b 2, :c 3 :d 4 :e 5})
;;=> [:a 1]
(rest {:a 1, :b 2, :c 3 :d 4 :e 5})
;;=> ([:b 2] [:c 3] [:d 4] [:e 5])
(conj {:a 1, :b 2, :c 3 :d 4 } [:e 5])
;;=> {:a 1, :b 2, :c 3, :d 4, :e 5}
(conj {:a 1, :b 2, :c 3 :d 4 } [:d 5] [:e 6])
;;=> {:a 1, :b 2, :c 3, :d 5, :e 6}
(conj {:a 1, :b 2, :c 3 :d 4 } [:d 5])
;;=> {:a 1, :b 2, :c 3, :d 5}
```

Maps are often used as dictionaries. get, dissoc, and assoc are more commonly used with maps.
