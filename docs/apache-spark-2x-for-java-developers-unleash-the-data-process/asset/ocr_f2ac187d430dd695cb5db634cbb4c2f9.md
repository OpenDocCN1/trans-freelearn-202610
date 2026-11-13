treated as a stream element; the decoding charset can also be defined while parsing the file:

```
//Streams from File
Stream<String>streamOfStrings = Files.lines(Paths.get("Apology_by_Plato.txt"
Stream<String>streamWithCharset = Files.lines(Paths.get("Apology_by_Plato.t>
```

Streams are usually pipelined to perform a set of operations that comprises of a source (collectionxns, IO channel), zero or more intermediate operations and a terminal operation.