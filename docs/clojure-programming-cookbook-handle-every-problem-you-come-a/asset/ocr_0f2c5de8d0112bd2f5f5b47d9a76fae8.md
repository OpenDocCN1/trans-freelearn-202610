We will let the reader check the code generated for each case with macroexpand-1, as usual:

```
(macroexpand-1
  ' (log "hello3"))
; (println "hello3")
```

# How it works...

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