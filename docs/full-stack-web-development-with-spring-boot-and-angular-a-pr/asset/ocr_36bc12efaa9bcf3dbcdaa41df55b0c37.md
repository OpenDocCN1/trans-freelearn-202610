+   - isEmpty(): Verifies whether the given iterable has a length greater than 0:

```
List test = new List();
assertThat(test).isEmpty();
```

+   - isNotEmpty(): Verifies whether the given iterable has a length of 0:

```
List test = List.asList("Thor", "Hulk",
    "Dr. Strange");
assertThat(test).isNotEmpty();
```

+   - hasSize(): Verifies whether the length of the iterable is equal to the given value:

```
List test = List.asList("Thor", "Hulk",
    "Dr. Strange");
assertThat(test).hasSize(3);
```

These are just some of the assertions available under AbstractIterableAssert. For the complete documentation, you can go to the link provided here: https://joel-costigliola.github.io/assertj/core-8/api/org/assertj/core/api/AbstractIterableAssert.html.

# File assertions

File assertions are used to verify whether a file exists, can be written, or is readable, and also verify its contents. The following are some of the file assertion methods:

+   - exists(): Proves that the file or directory exists:

```
File file = File.createTempFile("test", "txt");
assertThat(tmpFile).exists();
```

+   - isFile(): Verifies whether the given object is a file (providing a directory will result in a failed test):

```
File file = File.createTempFile("test", "txt");
assertThat(tmpFile).isFile();
```

+   - canRead(): Verifies whether the given file is readable by the application:

```
File file = File.createTempFile("test", "txt");
assertThat(tmpFile).canRead();
```