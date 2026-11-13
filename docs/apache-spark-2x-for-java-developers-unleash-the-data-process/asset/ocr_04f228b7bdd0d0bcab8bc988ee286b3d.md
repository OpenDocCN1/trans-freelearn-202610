# # Syntax of Lambda expressions

A Lambda expression consists of two parts: 0 or more arguments and the function body:

```
| argument -> function
(argument1,argument2,...argument) -> function
```

Let's discuss it with examples. Consider a list of integers as follows:

```
|List<Integer> list = Arrays.asList(1,2,3,4,5);
```

Print each element of the list using Lambda:

```
|list.forEach(n-> System.out.println(n));
```

Multiply each element of the list by 2:

```
|list.stream().map(n -> n*2 );
```

The stream method will convert the list to a stream of numbers. We will discuss more about it in another section. For now, just consider that the stream will convert all the elements of the list into the stream.

Here, we do not need to specify the type of n. The type of n is auto-inferred in Lambda. Also, we have not specified any return statement. The return statement is implicit in Lambda. If you want, you can specify the return statement explicitly as follows:

```
|list.stream().map(n -> {
    return n*2;
});
}
```

For specifying the return type explicitly, you need to enclose the function body in braces and provide a semicolon (;) at the end of the return statement.