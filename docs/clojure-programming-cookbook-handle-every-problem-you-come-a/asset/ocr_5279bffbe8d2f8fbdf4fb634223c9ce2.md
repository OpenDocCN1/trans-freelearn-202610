```
init
; 1449641148783

(up)
; 596928
```

This kind of macro, which generates code for you, can be very helpful during development, so be careful as this kind of macro can also leave some def-ed symbols about the place!

# Logging a message with macro

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