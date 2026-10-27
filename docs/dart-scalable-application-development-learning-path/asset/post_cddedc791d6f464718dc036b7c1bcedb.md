
The periodic timer handler `performUpdate` is provided a `Timer` object as a parameter when it is triggered every 10 seconds. When we are directly calling it, `null` is provided instead of the `Timer` instance.

### Performing the update

The two elements being updated are the current time, with a countdown to the next refresh, and the grid of the data. Let's have a look at the following code snippet:

```dart
void performUpdate(Timer triggerTimer) {
    DivElement outputDiv = querySelector('#timestatus');

    DateTime currentDateTime = new DateTime.now();
    DateFormat timeStamp = new DateFormat("hh:mm a");

    if (triggerTimer != null) {
        secondsToUpdate -= 10;
    } else updateDataView();

    if (secondsToUpdate == 0) {
        updateDataView();
        secondsToUpdate = 60;
    }

    outputDiv.text =
        "${timeStamp.format(currentDateTime)} - $secondsToUpdate seconds until refresh.";
}
```

The `secondsToUpdate` variable is decremented from 60 to 0 and triggers an update every six calls so that the display changes every minute.

### Fetching the JSON

The `updateDataView` function in the `main.dart` file covers fetching the data and updating the grid view:

```dart
...
HttpRequest.getString(jsonSrc).then((String data) {
    outputDiv.children.clear();

    List items;
    try {
        items = JSON.decode(data);
    } catch (exception, stackTrace) {
        print(exception);
        print(stackTrace);
    }
...
```

The `DivElement outputDiv` is the container for the dynamically updating content, which is entirely cleared at each update. The incoming JSON data is decoded and stored in the `List` object named `items`.

### Configuring the grid view control

The data for the grid is provided as a structure of a list of lists. The first list in the structure forms the header of the table. Let's have a look at the following code snippet:

```dart
...
if (items != null) {
    List allQuakeData = [];
    allQuakeData.add(
        ['Time', 'Magnitude', 'Type', 'Tsunami', 'Place', 'Sig', 'Link']);

    items.forEach((String post) {
        Map decodedData = JSON.decode(post);

        List quakeData = [];
        quakeData.add(convertTime(decodedData['properties']['time']));
        quakeData.add(decodedData['properties']['mag']);
        quakeData.add(decodedData['properties']['type']);
        quakeData.add(decodedData['properties']['tsunami']);
        quakeData.add(decodedData['properties']['place']);
        quakeData.add(decodedData['properties']['sig']);
        quakeData.add(decodedData['properties']['url']);

        allQuakeData.add(quakeData);
    });

    outputDiv.append(Gridview.getTable(allQuakeData));
}
...
```

The JSON items are iterated over and the values for the grid are extracted. The only value not used directly is the time field, which requires formatting.

The preceding code renders the following output display, which will vary according to the time at which it is executed:

Date and Time Demo

| Format | Output |
|--------|--------|
| hh:mm a | 04:12 PM |
| HH:MM | 16:06 |
| y | 2015 |
| d | 13 |
| EEEE | Saturday |

The function `printTime` formats the input `DateTime` to the desired style. The `padRight` method on the `String` object is used to make a simple table by adding spaces to pad out the string to 12 characters, as shown here:

```dart
void printTime(DateTime dt, String format) {
  DateFormat timeStamp = new DateFormat(format);
  print("\t${format.padRight(12)} ${timeStamp.format(dt)}");
}
```

The second part of the program allows a date to be entered by using the `stdin.readLineSync()` method. The user can enter a blank line to exit. The string is parsed according to the format `yyyy-MM-dd`, and the weekday is displayed if it is successfully parsed:

```dart
while (true) {
  String userDate = "";
  print("\nPlease enter a date (yyyy-MM-dd):");

  try {
    DateFormat timeStamp = new DateFormat("yyyy-MM-dd");
    DateFormat outputFmt = new DateFormat("EEEE");

    userDate = stdin.readLineSync();
    if (userDate.length == 0) exit(0);

    print(outputFmt.format(timeStamp.parse(userDate)));
  } catch (exception, stacktrace) {
    print('Error parsing the entered date.');
    print(exception);
    print(stacktrace);
  }
}
```

The parsing is wrapped in `try catch` to deal with an exception raised by a failed parse of a date string.

### Building the table

The class `GridView`, located in the file `lib/src/webgridview_base.dart`, prepares a table from the list of lists provided by the calling web page. The static method `getTable` performs the task. As it is static, an instance of the class is not required to call this function. Let's have a look at the following code snippet:

```dart
static TableElement getTable(List rows) {
  TableElement te = new TableElement();
  rows.forEach((row) => addRow(te, row));
  return te;
}
```

The `addRow` function iterates over the columns in the results. The header row is added as a regular row as the CSS will take care of the special formatting of the first row, as shown here:

```dart
static addRow(TableElement table, List cols) {
  TableRowElement tableRow = table.addRow();

  cols.forEach((column) {
    TableCellElement tableCell = tableRow.addCell();
    String content = column.toString();
    if (content.startsWith('http')) {
      content = "<a href=\"$content\">Link</a>";
      tableCell.appendHtml(content);
    } else tableCell.text = content;
  });
}
```

The content is added as text to the table cell, with one special case of strings that start with `http`. These URLs are formatted into HTML with a generic link text, and the `appendHtml` method is used to ensure that the text is not sanitized before being added to the page.

### Showing the page

Launching the application in Dartium will show the `index.html` page:

![](img/52c05b53a56abd53584e4fed738ed509_197_0.png)

Keep this page open over a period of time and the table will update smoothly as new data becomes available. Events from outside the USA do appear, though less often. It is fairly rare, but the Type column does not always read 'earthquake'.

### Summary

The applications created in this chapter have been numerous, but all work together to reach the goal of a live display of data.

The data collector transforms raw live data into a processable form. The REST API was created with the rpc package, taking much of the work out of the way. Creating a new web API method is a case of adding an annotated regular Dart method.

The consuming web application showed how to connect to a JSON standard web service and produce high-quality output, updated in real time. The HTML DOM was used to swap in a new table-based display.

The incoming data on earthquakes has proven quite fascinating to watch, but it could be more visual. In the next chapter, we’ll create a visualization using the feature geometry data stashed in the database, and look at expanding the API to do more than provide a data feed.

## 9 A Real-Time Visualization

> There's just something hypnotic about maps.
>
> - Ken Jennings

Maps are indeed hypnotic as are flashing indicator lights, which may give you a rough idea of where the earthquake monitor project is going next. It is the logical format to display all that rich geographical data we are collecting.

An animated display is one step up from a static display, and the step beyond animation is to make it interactive. We will look at accessing the web browser features from Dart in order to integrate to the desktop and work with the user's location.

To work in a professional development environment or to share with the community online, we will need to create documentation from our Dart source.

### Iteration overview

The grid view gave a very detailed view of the data obtained from the stack of software that collected the data from the web service, was stored in a database, and was then shared from the database via an API.

For this part of the project, we will reuse every part other than a `gridview`, and build a new visualization using a world map.
