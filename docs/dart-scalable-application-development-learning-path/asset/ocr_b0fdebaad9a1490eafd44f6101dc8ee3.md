```
```dart
exit(10);
}
}
}
```
```

You can also set the exit code value with the property `exitCode`, as shown in the following code:

```
```dart
exitCode = 10;
// ... other code can be executed
exit(exitCode);
```
```

# How it works...

The exit (code) function will terminate the running Dart VM process and return the integer code as the exit value to the parent process or OS environment, indicating the success, failure, or other exit state of the program. You can choose the value of the code; there is a convention to use 0 for success, 1 for warnings, and 2 for errors. Another convention is zero for success, non-zero for failure, and a program returning a warning for a successful exit because it naturally reached its end. A concrete example is the dartanalyzer program, which returns 0 if the code generates warnings.

Setting the exit code is preferred because the program can still run to its natural completion, or some cleanup or finalizing code (such as closing a file or database connection) can be run before `exit(exitCode)` ends the app. It is also good practice to start `main()` with `exitCode = 0`, presuming success as the normal ending state.

> What exit codes mean is platform-specific; you will not run into cross-platform issues if you use exit codes in the range of 0–127.

# Parsing command-line arguments

A server app that runs a batch job often takes parameter values from the command line. How can we get these values in our program?

# How to do it...

The obvious way to parse command-line arguments is as follows (see `command_line_arguments.dart`):

```
```dart
void main(List<String> args) {
  print("script arguments:");
```
```