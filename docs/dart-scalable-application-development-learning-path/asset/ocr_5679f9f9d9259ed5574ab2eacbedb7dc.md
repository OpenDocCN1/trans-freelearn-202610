```
print(data);
// [228, 189, 160, 229, 165, 189, 228, 184, 150, 231, 149, 140]
print(UTF8.decode(data)); // prints '你好世界'.
});
```

4. We can write to a file using openWrite as shown in the following code:

```
var file = new File('out3.txt');
var sink = file.openWrite();
sink.write('File was written to at ${new DateTime.now()}\n');
// close the IOSink to free system resources!
sink.close();
}
```

# How it works...

Step 1 uses the asynchronous writeAsString method to write one (big) string to a file, and this file is then automatically closed. In the callback function called by then, you could, for example, send the file over the network. Step 2 shows how to write raw bytes to a file with writeAsBytes. This is necessary when the file contains non-readable or Unicode characters.

# There's more...

What do we do when we want to write to our file in chunks? Then, we use the openWrite method as shown in step 3. When called on a File object, this creates an IOSink object for that file, which you can write to with any of the following methods: write, writeln, writeCharCode, writeAll. In contrast to the write methods of the previous steps, the IOSink object must be explicitly closed when no longer needed. The openWrite method takes two optional arguments as shown in the following code:

```
file.openWrite(mode: FileMode.APPEND, encoding: ASCII);
```

The default mode is FileMode.WRITE.

# Searching in a file

In this recipe, we demonstrate how to search for certain words in a text file. You can find the code in the search.dart script in the project searching_file. As an example text file, we use taoprog.txt.