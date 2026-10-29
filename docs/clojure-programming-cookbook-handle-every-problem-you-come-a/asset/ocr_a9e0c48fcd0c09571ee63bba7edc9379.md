# Summary of array accesses

In the following table, we summarize Java's array accesses from Clojure and equivalent code using Java:

| Action | Clojure | Java |
|---|---|---|
| Creating a one-dimensional array | (long-array 10) | new long[10]; |
| Creating a two-dimensional array | (make-array Double/TYPE 10 10) | new Double[10][10]; |
| Getting an element of an array | (aget str_array 0) | str_array[0]; |
| Setting a value to an element of an array | (aset str_array 0 "x") | str_array[4] = "x"; |

# Print strings of Java's primitives

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