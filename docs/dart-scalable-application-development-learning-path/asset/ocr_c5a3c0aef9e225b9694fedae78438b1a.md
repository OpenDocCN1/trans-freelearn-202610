This is called by the presentation overview display without requiring any parameters:

```
buildOverview();
```

This is called by the handouts display without requiring any parameters:

```
buildOverview(true);
```

If this parameter is set, an additional Div element is added for the Notes area and the CSS is adjusted for the benefit of the print layout.

# Comparing optional positional and named parameters

The addNotes parameter is declared as an optional positional parameter, so an optional value can be specified without naming the parameter. The first parameter is matched to the supplied value.

To give more flexibility, Dart allows optional parameters to be named. Consider two functions—the first will take named optional parameters and the second will take positional optional parameters:

```
getRecords1(String query,{int limit: 25, int timeOut: 30}) {
}
```

```
getRecords2(String query,[int limit = 80, int timeOut = 99]) {
}
```

The first function can be called in more ways:

```
getRecords1("");
getRecords1("", limit:50, timeOut:40);
getRecords1("", timeOut:40, limit:65);
getRecords1("", limit:50);
getRecords1("", timeOut:40);
```

```
getRecords2("");
getRecords2("", 90);
getRecords2("", 90, 50);
```

With named optional parameters, the order they are supplied in is not important and has the advantage that the calling code is clearer as to the use that will be made of the parameters being passed.