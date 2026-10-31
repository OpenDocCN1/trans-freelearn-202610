
3. Here, a file `info.txt` will be opened in `Notepad`.

4. You can store the `shell` command in a `word`.

5. In the previous cases, the `Red` app continues. If you want it to wait until the started process has finished, use `/wait`.

6. Write the output of the command to the given file (here, this is on Linux/OS X). The output could also be a variable.

7. Any valid `shell` command can be given to `call`. Here, we use `start` on Windows to create an empty file.

The `call` value returned is either the process-id of the started program, or else `0` when the startup is successful, and `-1` when an error occurs.

# Using the OS clipboard – read-clipboard

The following code temporarily stores some information to the clipboard:

```
write-clipboard "Some important info" ;== true
; close Red console or Red program
print read-clipboard ;== Some important info
```

This can be used as a means of communication between two `Red` apps. The info stays on the clipboard until the OS restarts or until another program writes to the clipboard.

# Datatypes for performance

We all love and know our trusted `series!`, but certain apps need all the performance they can get, especially when dealing with large datasets. For that specific reason, the `vector!`, `map!`, and `hash!` datatypes were constructed.

## The vector! datatype

When your `series!` contains values of one of the `integer!`, `float!`, `char!`, or `percent!` types and only one of these, and you need fast mathematical operations, then you want to use the `vector!` type, like this:

```
;-- see Chapter10/perf-datatypes.red:
v1: make vector! [7 13 42 108]
vector? v1       ;== true
```

In a `vector!`, all values need to be of the same type. You can perform simple arithmetic with vectors like so:

```
v1 + 2       ;== make vector! [9 15 44 110]
print v1     ;== 9 15 44 110
v1 * 4       ;== make vector! [36 60 176 440]
v2: make vector! [1 2 3 4]
v1 + v2      ;== make vector! [37 62 179 444]
```

Note that the operations on one vector change it in place, that means the vector `v1` in our examples is changed by the operations. Most actions that we know from `series!` can be used with vectors. Here is an example:

```
v1: make vector! [7 13 42 108]
length? v1 ;== 4
append v1 666 ;== make vector! [7 13 42 108 666]
find v1 42 ;== make vector! [42 108 666]
; looping through a vector:
foreach n v1 [prin [n "-"]] ;==7 - 13 - 42 - 108 - 666 -
v1/3 ;== 42 
```

## The map! and hash! datatypes

Many languages have a dictionary type to efficiently store values associated with keys, and to provide fast lookup for those keys—for example, `key1` -> `value1`, `key2` -> `value2`, and so on. `Red` offers two datatypes for this, the `hash!` and the `map!`.

With `hash!`, you get a type that has all the navigational functionality that exists for `series!`. When the key values are simple types, they get hashed, which results in a *fast value lookup*. First, you define the key, then its value, then a new key with its value, and so on:

```
h1: make hash! [n 13 m 42 o 108]
h1/m           ;== 42
select h1 [m]  ;== 42 
select h1 'm   ;== 42

hash? h1       ;== true
hash? [n 13 m 42 o 108]   ;== false
```

Value lookup is done with the path notation `hash/` `key` or through `select`, with the key represented either in a block or as a literal word. This datatype is especially suited for dealing with large datasets in which you have to constantly search. The `hash!` word can also be used without any key-values, just to provide a `series!` with fast lookup, like this:

```
fruits: ["apple" "ananas" "tomato" "strawberry" "grap"]
to-hash fruits  ;== make hash! ["apple" "ananas" "tomato" "strawberry" "grap"]
pick fruits 3   ;== "tomato"
```

The `map!` type is the preferred type for a high-performance dictionary using a hash table, but in contrast to `hash!`, it's not of the `series!` type, so, for example, it doesn't know about index position or offset:

```
m1: make map! ["Red" 1 "Crystal" 7 "Python" 13 "Ruby" 42]
== #(
 "Red" 1
 "Crystal" 7
 "Python" 13
 "Ruby" 42
)
```

However, we can see that it has a convenient special syntax, which can also be used to create a map:

```
m5: #(
    "Red" 1
    "Crystal" 7
)
type? m5 ;== map!
```

To get a value from a map, use `select`. To insert a new value or modify an existing value, use `put`. To delete a key-value pair from the map, use `put` `key` `none`:

```
select m1 "Crystal"     ;== 7
put m1 "Crystal" 5      ;== 5
put m1 "Java" 132       ;== 132
put m1 "Ruby" none      ;== none ; delete key "Ruby" and its value 
print m1
; output:
"Red" 1
"Crystal" 5
"Python" 13
"Java" 132
```

Use `find` to check whether a map contains a certain key. If it does, it returns `true`; otherwise, it returns `none`. The `length?` word returns the number of key-value pairs. Use `keys-of` and `values-of` to get respectively the keys and values of a map in a block form:

```
find m1 "Crystal" ;== true
find m1 "C#"      ;== none
length? m1        ;== 4
keys-of m1        ;== ["Red" "Crystal" "Python" "Java"]
values-of m1      ;== [1 5 13 132]
body-of m1        ;== ["Red" 1 "Crystal" 5 "Python" 13 "Java" 132]
```

All this functionality makes `map!` the type you should choose to build a dictionary data structure with.

Keys can be of the `any-word!`, `any-string!`, `scalar!`, and `binary!` datatypes.

When the keys are words, the familiar `/` path notation can be used to retrieve and set values:

```
m4: make map! [n 13 m 42 o 108]
m4/m ;== 42
m4/m: 43
print m4
; output:
n: 13
m: 43
o: 108
```

When the keys are of another type, the key value has to be enclosed in `( )` in order for the path notation to work. A series of key-value pairs can also be merged with an existing map by using the `extend` word:

```
extend m4 [p 113 q 666]
print m4
;== #(
    n: 13
    m: 43
    o: 108
    p: 113
    q: 666
)
```

The same word can also be used to extend an object with new properties and values.

By default, lookup and modification is case-insensitive—that is, the keys `Red`, `red`, and `RED` are considered the same. If you want to work in a case-sensitive way, use `put/case` and `select/case`.

One of the predefined maps is `list-env`, which gives you a map of OS environment variables (for the current process). You can read out the values of these variables as follows:

```
select list-env "windir"   ;== "C:\WINDOWS" ; on Windows
select list-env "SHELL"    ;== "/bin/bash"
```

=> Try this out in question 5 from the *Questions* section.

# Embedded Red and macros

In this short section, we will touch on some advanced topics in `Red` that you should know about. We'll give you references to more detailed descriptions in case you need them for your projects.

## Embedded Red

Software written in languages other than `Red` can interact with `Red` through `libRed`, a special version of the `Red` interpreter and runtime library. That way, for example, a `C` program can interact with a `Red` environment. Bindings also exist for `Visual Basic`, `Ruby`, and `Julia` ([https://github.com/joa-quim/Red.jl](https://github.com/joa-quim/Red.jl)). Detailed docs are at [https://doc.red-lang.org/en/libred.html](https://doc.red-lang.org/en/libred.html) and [https://github.com/red/red/wiki/libRed](https://github.com/red/red/wiki/libRed) [for additional examples.](https://github.com/red/red/wiki/libRed)

## Macros

Another feature many modern programming languages have is macros. Macros are about transforming code at runtime. By now, you should be convinced that `Red` is already well equipped to do that with minimal effort; think about all you can do with `do`, `reduce`, and `load`. After all, code is data and data is code! Because `Red` is first of all a compiled language, macros in `Red` are more oriented towards transforming code at compile time, but they work equally well when interpreted. Here is an example of a macro to give you a taste:

```
;-- see Chapter10/macros.red:
#macro as-KB: func [n][n * 1024]

print as-KB 64 ;== 65536   
```
