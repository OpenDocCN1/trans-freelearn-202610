
## How to do it...

The change we make in this recipe is nearly transparent to the user; the web page stays the same, but because making an HTTP request is asynchronous, we will work with a Future and must provide a message, such as "Loading data...", as long as the request is being executed.

- 1. In our JobListingController controller class, lib\job_listing.dart, we define a new variable of the type Http: final Http _http;. The constructor now becomes the following:

```
JobListingController(this._http) {
  _loadData();
}
```

The bulk of the change takes place in its _loadData() method, as shown in the following code:

```
static const String LOADING_MESSAGE = "Loading jobs listing...";
static const String ERROR_MESSAGE = "Sorry! The jobs database cannot be reached.";
String message = LOADING_MESSAGE;

void _loadData() {
  jobsLoaded = false;
  _http.get('jobs.json')
    .then((HttpResponse response) {
      jobs = response.data.map((d) => new Job.fromJson(d)).toList();
      jobsLoaded = true;
      for (var job in jobs) { // extract companies:
        companies.add(job.company);
      }
    })
    .catchError((e) {
      print(e);
      message = ERROR_MESSAGE;
    });
}
```

In the class Job we have a new named constructor, as follows:

```
Job.fromJson(Map<String, dynamic> json) : this(json['type'], json['salary'], json['company'], DateTime.parse(json['posted']), json['skills'], json['info']);
```

## How it works...

In step 1, we see that Angular uses dependency injection when instantiating the controller to supply it with an `Http` object. Step 2 shows how the `get` method from the `Http` service is used to make a GET request and to fetch data from the server (here the data is local, but it could be fetched from a remote site). This method returns a Future `<HttpResponse>` when the request is fulfilled. That's why we have to use a `.then` construct to register a callback function and a `.catchError` to handle exceptions. In the callback function, we have the data in `response.data`. With map, we call for each job as a JSON string. The named constructor `fromJson` in the class Job from step 3 to transform it into a Job object. Finally, a list is made with all job data, which is bound to the view in the same way as in the previous recipes.

## See also

- For more information about Futures, refer to *Chapter 8, Working with Futures, Tasks, and Isolates*
- For more details about JSON HTTP requests, refer to the *Downloading a file* recipe in *Chapter 6, Working with Files and Streams*

## Deploying your app

To run in any modern browser, your Angular app has to be compiled to minimal JavaScript. Minimal means tree-shaken (so that unused code is left out) and minified (shortening of names and minimum use of spaces).

But first, test your app in other browsers by performing the following steps:

- 1. First, test in your default browser by right-clicking on the startup web page and selecting **Run as JavaScript**. This will compile to JavaScript and execute the app in the browser, but the compiled code will be kept in memory and not written to disk.
- 2. To change browsers, go to the menu-item and navigate to **Tools | Preferences | Run | Debug**, uncheck **Use system default browser**, and select the other browser. You can also just start that browser and copy the URL from the app in your default browser.

## How to do it...

- 1. Include the Angular transformer into the app's `pubspec.yaml` file:
  
      ```yaml
      transformers:
        - angular
      ```
- 2. Also, add the `js` and `shadow_dom` packages to your `pubspec.yaml` file.

## How it works...

3. Add the shadow DOM script to the startup HTML file, as follows:

```
<script src="packages/shadow_dom/shadow_dom.min.js"></script>
```

4. Use pub build either from the command line or from Dart Editor.

Step 2 will make sure that Angular can work with Shadow DOM in all browsers (for more information on Shadow DOM, refer to the How it works section in the Setting up an angular app recipe in this chapter).

> Instead of shadow_dom.min.js, you can use shadow_dom.debug.js while debugging.

Step 3 will generate all deployable files in your app's directory in a subfolder named build/web. For example, if we apply this to the angular_view app, we get a JavaScript file, angular_view.dart.js, of about 1.7 MB to start up together with angular_view.html. The following screenshot gives us an overview of the angular_view project:

![](img/52c05b53a56abd53584e4fed738ed509_866_0.png)

Overview of the project layout

## See also

- ▸ You can learn more about transformers at `https://www.dartlang.org/tools/pub/assets-and-transformers.html`. For more information about compiling to JavaScript, see the *Compiling your app to JavaScript* recipe in *Chapter 1, Working with Dart Tools* and the *Publishing and deploying your app* recipe in *Chapter 2, Structuring, Testing, and Deploying an Application*.

## Bibliography

This Learning Path is a blend of content, all packaged up keeping your journey in mind. It includes content from the following Packt products:

- *Dart By Example By Davy Mitchell*
- *Mastering Dart By Sergey Akopkokhyants*
- *Dart Cookbook By Ivo Balbaert*

![](img/52c05b53a56abd53584e4fed738ed509_871_0.png)

## Thank you for buying
Dart Scalable Application Development

## About Packt Publishing

Packt, pronounced 'packed', published its first book, *Mastering phpMyAdmin for Effective MySQL Management*, in April 2004, and subsequently continued to specialize in publishing highly focused books on specific technologies and solutions.

Our books and publications share the experiences of your fellow IT professionals in adapting and customizing today's systems, applications, and frameworks. Our solution-based books give you the knowledge and power to customize the software and technologies you're using to get the job done. Packt books are more specific and less general than the IT books you have seen in the past. Our unique business model allows us to bring you more focused information, giving you more of what you need to know, and less of what you don't.

Packt is a modern yet unique publishing company that focuses on producing quality, cutting-edge books for communities of developers, administrators, and newbies alike. For more information, please visit our website at www.packtpub.com.

## Writing for Packt

We welcome all inquiries from people who are interested in authoring. Book proposals should be sent to author@packtpub.com. If your book idea is still at an early stage and you would like to discuss it first before writing a formal book proposal, then please contact us; one of our commissioning editors will get in touch with you.

We're not just looking for published authors; if you have strong technical skills but no writing experience, our experienced editors can help you develop a writing career, or simply get some additional reward for your expertise.
