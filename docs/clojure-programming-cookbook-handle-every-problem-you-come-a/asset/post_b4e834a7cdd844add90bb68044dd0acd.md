
It makes more sense with an example, so let's do it:

```
(defmacro hello2d [arg]
  `(+ 1 ~arg))
```

We are now introducing two different characters: unquote, or ~, and unquote splicing, or ~@. Think of unquote and unquote splicing as copy and paste, where you can insert the form directly in the generated code. So in the preceding code, the generated code should expand to the actual value of the arg, and not simply the arg string:

```
(macroexpand-1 '(hello2d 3))
; (clojure.core/+ 1 3)
```

Wow! This feels way better, doesn't it. Let's just confirm if it can be evaluated properly:

```
(hello2d 3)
; 4
```

Finally, unquote splicing makes sense with list, maps, and so on, but the copy/paste idea stays the same. One copy and pastes the list as is, while the other copies the forms one by one into the template:

```
(defmacro hello2e [& body ]
  `(apply str ~body))
```

```
(defmacro hello2f [& body ]
  `(apply str ~@body))
```

```
(macroexpand-1 '(hello2e "1" "2" "3"))
; (clojure.core/apply clojure.core/str ("1" "2" "3"))
```

```
(macroexpand-1 '(hello2f  "1" "2" "3"))
; (clojure.core/apply clojure.core/str "1" "2" "3")
```

## Macros everywhere

In this recipe, we have seen how to create our very first macro, and moved along to grasping what compiled code is and when it occurs in the code lifecycle. We saw that Clojure was a homogenic language,

meaning its internal structure and its representation were the same, and we linked this homogenic feature and macro concept together.

## Defining more advanced macros

When we started thinking about the structure of the book, it all kind of made sense to talk about simple macros and advanced macros. Now, arriving at the topic, the author has to admit it is not so clear anymore. So, what do we mean when we talk about advanced macros? We tried to ask Canon, but they would not give us a proper answer.

In this recipe, we will therefore state that an advance macro is a macro that generates a form that changes the flow of the evaluation of the resulting code. In the previous recipe on simple macros, the macros were, well, simple, because the form generated was always the same. This recipe will expose macros that always generate the same code or where the control flow gets slightly more complicated. The goal of course is to easily be able to write convenient macro in your code.

### Getting ready

While there is no specific preparation for this recipe, let's have a look at the clojure.core when we have a macro to guide us:

```
(defmacro when
  "Evaluates test. If logical true, evaluates body in an implicit do."
  {:added "1.0"}
  [test & body]
  (list 'if test (cons 'do body)))
```

With what we learned, we can rewrite it in a slightly more concise form with the backquote character:

```
(defmacro my-when
  [test & body]
  `(if ~test (do ~@body)))
```

The code generated is according to the plan:

```
(macroexpand-1
  '(my-when (odd? (rand-int 2))
```

```
(println "hello")))
; (if (odd? (rand-int 2)) (do (println "hello")))
```

Good. Now the thing we are interested in doing for advanced macros is to generate more advanced code. So, following through this example, we will write a macro that randomly generates code.

The macro in the following code does not take a test as an input parameter anymore, just a body. We mostly moved code around, but look! The unquote symbol is no longer at start of the body of the macro, it is now nested inside the if statement.

Remember the macro body needs to return a Clojure form. The macro in the following code will randomly return either the passed body list wrapped in a do, or an empty list:

```
(defmacro random-when
  [& body]
  (if (odd? (rand-int 2))
    `(do ~@body)
    (list)
    ))
```

Try the following a few times in your REPL and you will see that the code generated is not always the same:

```
(macroexpand-1
  '(random-when
     (println "Hello")))
; ()
; OR
; (do (println "Hello"))
```

A very silly example it is, but that should open your mind to new tracks and adventures for ever more advanced macros!

### How to do it...

We will look at two small examples to see how some slightly advanced macros can facilitate your developer's life and make your evening glass of wine easier to grasp.

### Tracking when code was started

The first sample will be a macro that generates things for us, the time `init` was called, and a function that tells us how long the system has been up.

The macro is making use of two calls to `def` - one for a simple value and the other for a function:

```
(defmacro track-init[]
  `(do
    (def init (System/currentTimeMillis))
    (def up (fn [] (- (System/currentTimeMillis) init))))
  )
```

The call to the Java platform's `System` class is a bit overwhelming, but basically we just want to access the current time. Expand the generated code as follows:

```
(macroexpand-1
  '(track-init))
```

We see what looks like standard Clojure, defining one value and a function:

```
(do
  (def my-very-first-macro/init
    (java.lang.System/currentTimeMillis))
  (def my-very-first-macro/up
    (clojure.core/fn []
      (clojure.core/-
        (java.lang.System/currentTimeMillis)
        my-very-first-macro/init))))
```

Except you did not write this code, the only thing you wrote in the first place was that call to `track-init` macro:

```
(track-init)
```

Now, in the namespace, the macro was called, therefore in the same namespace we have `init` and `(up)` :

```
init
; 1449641148783

(up)
; 596928
```

This kind of macro, which generates code for you, can be very helpful during development, so be careful as this kind of macro can also leave some def-ed symbols about the place!

### Logging a message with macro

In the second example, we will simply look at a dumb log macro that gives an output message when the log level is sufficient and doesn't if not. Umm. Yes. I miss the days of 20 logging frameworks to rule none of them. (But that is history.)

So, the following defines a top log level that could be automatically loaded from a file, but I leave that to the reader. We also define the log macro that gives output of the code only if the log level is set to DEBUG:

```
(def LOG_LEVEL "INFO")

(defmacro log [msg]
  (if (= LOG_LEVEL "DEBUG")
    `(println ~msg)))
```

In this case, when we call the macro using the following statement:

```
(log "hello3")
; ...
```

You got it right. Nothing is printed in the console. Now, let's say we def the log level again, (hmmm ... this is not the recommended way of doing things but bear with us here):

```
(def LOG_LEVEL "DEBUG")
```

When calling the log macro again, messages are shown in the console:

```
(log "hello3")
; .. hello3 in the console
```

We will let the reader check the code generated for each case with macroexpand-1, as usual:

```
(macroexpand-1
  ' (log "hello3"))
; (println "hello3")
```

### How it works...

I guess we have seen quite a bit on what the underlying process is when playing with macros. Let's have a look at one of the macros that is all over the Internet to create an iterative language looking for loop.

The code for the loop macro is only slightly more difficult than what you have seen so far. Notice how the return form of the macro is a list, through backquote, and how the body steps are wrapped in a do:

```
(defmacro my-for-loop [ [sym init check change] & steps]
  `(loop [~sym ~init value# nil]
     (if ~check
       (let [new-value# (do ~@steps)]
         (recur ~change new-value#))
       value#)))
```

The one new thing you may have noticed is the # sign here and there, for example, new-value#. The sharp sign is an indication that you would like to get a generated, qualified name for a variable or a function:

```
gensym is the longer version of this #.
(gensym "hello")
; hello6075
```

If we were not using gensym or #, we would try to get the value of, say, new-value, which is not defined when the macro code is generated and thus would throw an error.

To generate your own loop, you would then call the macro as defined, with four parameters and a body:

```
(my-for-loop
  [ i 0 (< i 10) (inc i) ]
```
