You may find working in Dartium to be an odd experience. It is a good browser, but you may prefer your daily web browser, such as Internet Explorer, Firefox, or Chrome, with your carefully-managed settings and collection of extensions. Using Dart-to-JavaScript (dart2js) before testing on other browsers interrupts the edit and refresh cycle. Ideally, Dart would run in any web browser.

> Dartium is still going to be around for a long time; however, there is a project underway by the Dart team called the Dart development compiler. This new compiler has two goals, the first being to allow a smooth edit and refresh development experience on all browsers. A modular (incremental) compilation is used to aid a fast edit/refresh cycle. Find out more about the compiler project at https://github.com/dart-lang/dev_compiler.
> The second goal of the compiler is to create a human-readable JavaScript format so that the output can easily be debugged. Initially supporting subset of the Dart language that is statically type checked. The type system has a different approach to dart2js that is to allow more direct and readable mapping of the Dart language to JavaScript. If you are developing in Firefox, then you will want to be able to debug your Dart code in it so that you do not have to step through the obfuscated code.

# Exporting to CSV

CSV is a format that just keeps going thanks to its easy import into spreadsheets. The implementation of this report is found in the report_csv.dart file, and it retrieves the 50 most recent entries using the following URL:

```
http://127.0.0.1:8080/api/quake/v1/recent/50
```

> CSV is a text format, and the fields are cleared of any comma characters so as not to have extra delimiters.

Let's consider the following code:

```
performExport(MouseEvent event) async {
  String json = await HttpRequest.getString(jsonSrc);
  List<String> items = JSON.decode(json);
  String csvexport = "Type, Time, Place, Magnitude\r\n";

  items.forEach((String featureJSON) {
    Map feature = JSON.decode(featureJSON);
    String type = feature['properties']['type'];
```