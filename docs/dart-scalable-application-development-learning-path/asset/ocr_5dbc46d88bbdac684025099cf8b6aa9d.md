For more advanced handling of command-line arguments, see the very powerful args package on pub at https://pub.dartlang.org/packages/args.

readAsLines is an asynchronous function that immediately returns a Future object. A function is passed into the future object's then method. The timing of the execution of this function is unknown to the developer. The flow of the program continues straight away. When the file read operation is run and has completed, the function passed to the then method is executed when the Dart VM schedules it.

To see this in action, move the print statements outside of the then function, as shown, and run the program again:

```
myFile.readAsLines().then((List<String> Lines) {
    codeScan.scan(Lines);
});
print("${codeScan.totalLines}");
print("${codeScan.classes}");
print("${codeScan.comments}");
print("${codeScan.imports}");
print("${codeScan.whitespace}");
```

The probable output is all zeros as the code lines have not been scanned yet; this is because the print functions are being called first.

It is also possible that the program will run just as before. The key point is that the overall program execution keeps moving forward even when the code is asked to perform a potentially long-running task, allowing other tasks to continue and keeping the application responsive.

# Debugging the command-line program

If you launch the program as it is in WebStorm by using Run, then it will open up the debugger with a RangeError exception. This is because the program is assuming that a command-line argument has been passed and the program has been run without any input.