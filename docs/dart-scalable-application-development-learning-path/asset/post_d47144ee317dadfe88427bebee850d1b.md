
Hopefully, this can help you in improving your geographical knowledge and learning more about Dart!

When debugging in Dartium using a REST or other similar web service, it is possible to get extra logging in the console window of the browser's developer tools for XMLHttpRequests.

Bring up the context menu (right-click), and there is an option named Log XMLHttpRequests that displays the HTTP requests made by the web page in the console output window:

[-121.5371704, 36.8165016, 0.93]
129.91739911111114 177.278328
2.2222222222222223 3.3333333333333335
maxWidth 2
XHR finished loading: GET "http://127.0.0.1:8080/api/quake/v1/latest".

This can be invaluable when debugging a project and saves adding logging to every part of the code that makes an HTTP request.

## Zooming into the display

The canvas element has a range of features for 2D drawing, and this includes a scaling feature that we can use to hone in on the most active part of the map, North America. In main.dart, a flag exists called zoomed, defaulting to false, that aids the application in keeping track of the current display. A button is bound to the zoom code implementation:

```dart
zoomMap(Event evt) async {
  if (zoomed == true) {
    zoomed = false;
    quakeMap.mapCtx.resetTransform();
    quakeMap.mapCtx.scale(1, 1);
  } else {
    zoomed = true;
    quakeMap.mapCtx.scale(2, 2);
  }
  quakeMap.drawMapGrid();
}
```

## A Real-Time Visualization

The canvas is toggled between scaling settings, with a critical call to the method `resetTransform`. Without this, the scaling would not be back to normal. The entire map view must be redrawn immediately so that the user has a responsive experience. Let's have a look at the following screenshot:

![](img/52c05b53a56abd53584e4fed738ed509_210_0.png)

If you watch this page for a while, you will find the California and Alaska feature embedded heavily in the data. As the pop-up code handles the change in scale, these are still available. Pressing the **Zoom** button a second time returns to the full world view.

> The print function is not just for command-line applications but can be used in web applications, too. The output appears in the **Developer Tools** console log window, which can be displayed by right-clicking on the page to bring up the context menu and then clicking `Inspect Element`.

## A Real-Time Visualization

Once permission has been granted, the final step is to create an instance of the notification class. No further method call is required:

![](img/52c05b53a56abd53584e4fed738ed509_212_0.png)

The application is set to notify quite often—you may wish to raise the level at which quakes are notified or you will find yourself dismissing a lot of notifications. The visual display of the notification may vary on different platforms and web browsers.

## Plotting the user's location

Most mobile devices are fitted with a geolocation device, or the location can be determined using secondary information such as IP or even falling back to user input. This is exposed for the modern web developer to use in applications via the HTML5 geolocation API at `http://www.w3.org/TR/geolocation-API/`.

As with the desktop notifications, the user must give permission for the page to use their location. A further little hurdle is that location features do not work on Dartium. To try out this feature of the application, use pub build to create the JavaScript version and use another browser:

![](img/52c05b53a56abd53584e4fed738ed509_212_1.png)

The **Locate** button on the page will attempt to obtain the user's position and draw it on the page by adding it to the `featurePlotter` class instance:

```dart
locateUser(Event evt) async {
  Geoposition geoPos = await window.navigator.geolocation.
  getCurrentPosition();
  MapIndicator mapIndicator;
  var pos =
  featPlotter.toPoint(geoPos.coords.longitude, geoPos.coords.
  latitude);
  mapIndicator = new MapIndicator(pos.x, pos.y, quakeMap.mapCtx, 4);
  mapIndicator.colorPrimary = "#0000ff";
  mapIndicator.colorSecondary = "#00ffff";
  featPlotter.userLocation.add(mapIndicator);
}
```

The coordinates are converted in the same manner as the quakes, and the indicator is changed to a blue color so that it will stand out on the map, as shown in the following screenshot:

![](img/52c05b53a56abd53584e4fed738ed509_213_0.png)

Once the application is compiled to JavaScript, the geolocation data is made available with user permission and the blue dot indicates the user's location, which in this instance is the relatively earthquake-free United Kingdom.

The Dart VM that runs in the command line and Dartium is not the only manifestation. The Dart team is working on two experimental versions of Dart in other contexts that vary significantly from the main Dart VM. Currently, both operate in a mobile context and focus on concurrency and high performance.

Fletch runs on desktop OSs, but is intended for Android and iOS execution with no JIT. Fletch takes a somewhat opposite approach to the highly asynchronous Dart, favoring user-level threads that are blocked but holding onto just a few resources. Fletch offers an interesting prospect for sharing the same code on different platforms; this would be a major productivity boost for mobile developers.

https://github.com/dart-lang/fletch

Sky is Android, only at the time of writing, and ambitiously aims to have highly responsive applications running at 120 FPS. To achieve this, it prioritizes the user interface so that it can keep an 8 ms response even when other processing is taking place. Currently, it can potentially run wherever the Dart VM can.

https://github.com/domokit/sky_engine/tree/master/sky/packages/sky

The Fletch and Sky prototypes, if successful, are likely to be mobile or server virtual machines and not embedded in a web server.

## Sorting the feature list

So far, no use has been made of the third piece of data in the coordinates list, which is the depth of the quake. This data will be presented in the form of a sorted list of the current features when the user presses the Sort button. Let's have a look at the following code snippet:

```dart
void sortFeatures(Event evt) {
  featPlotter.sortFeatures();
  DivElement out = querySelector('#depthDetail');
  out.nodes.clear();
  featPlotter.geoFeatures.forEach((feature) {
    LIElement detail = new LIElement();
    detail.innerHtml = "${feature[5]}km - ${feature[3]}";
    out.nodes.add(detail);
  });
}
```

## Documenting Dart code with dartdoc

Documentation is a critical part of software development, and to encourage good documentation of packages, Dart has the tool dartdoc (https://github.com/dart-lang/dartdoc/), which creates static HTML documentation based on specially formatted code comments. Previous documentation tools for Dart were part of the SDK, while dartdoc is a separate project developed by the Dart team.

It can be installed using pub at the command line:

```bash
pub global activate dartdoc
```

We will take a look at a Dart package project to explore the features of dartdoc. Open the project quakerecord in the WebStorm Editor, and take a look at the file quakerecord.dart:

```dart
library quakerecord;
export 'src/quakerecord_base.dart';
```

This package exports a single file, quakerecord_base.dart, and in this file, the class Processor is declared:

```dart
/// A lightweight object to process a raw earthquake feature.
///
/// * Must have valid JSON.
/// * Must be less than 128MB.
///
class Processor {

// Store for the JSON.
String _feature;

/// The state of processing
bool get processed => true;

/// The default constructor.
///     Processor processor;
///     processor = new Processor('{}');
Processor(String feature) {}

/// Returns a converted object based on the feature passed into
/// the constructor [Processor].
object convert() {
    // A normal comment.
```
