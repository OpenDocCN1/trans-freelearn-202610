## JavaScript

**JavaScript** (also known as **JS** or **JScript**) is a programming language used to program HTML and web applications that run in the browser. JavaScript is mostly preferred for adding dynamic features and providing user-based interaction inside web pages. JavaScript, HTML, and CSS are among the most-used web technologies, and now they are also used with headless browsers (you can read more about headless browsers at `https://oxylabs.io/blog/what-is-headless-browser`). The client-side availability of the JavaScript engine has also strengthened its usage in application testing and debugging.

`<script>` contains programming logic with JavaScript variables, operators, functions, arrays, loops, conditions, and events, targeting the HTML **Document Object Model (DOM)**. JavaScript code can be added to HTML using `<script>`, as seen in the following code, or can also be embedded as a file:

```html
<!DOCTYPE html>
<html>
<head>
    <script>
        function placeTitle() {
            document.getElementById("innerDiv").innerHTML =
                "Welcome to WebScraping";
        }
    </script>
</head>
<body>
    <div>Press the button: <p id="innerDiv"></p></div>
```

## AJAX

AJAX is a web development technique that uses a group of web technologies on the client side to create asynchronous web applications.

JavaScript XMLHttpRequest (XHR) objects are used to execute AJAX on web pages and load page content without refreshing or reloading the page. Please visit the AJAX page on W3Schools (https://www.w3schools.com/js/js_ajax_intro.asp) for more information on AJAX. From a scraping point of view, a basic overview of JavaScript functionality will be valuable to understand how a page is built or manipulated, as well as to identify the dynamic components used.

> **Important note**
> 
> Please visit https://developer.mozilla.org/en-US/docs/Web/JavaScript, https://www.javascript.com/, https://www.w3schools.com/js/js_intro.asp, and https://www.w3schools.com/js/js_ajax_intro.asp for more information on JavaScript and AJAX.

We have learned about a few JavaScript-based techniques and technologies that are commonly deployed in web development today. In the next section, we will learn about data-storing objects.

## JSON

JSON is a format used for storing and transporting data from a server to a web page. It is language-independent and preferred in web-based data interchange actions due to its size and readability. JSON files are files that have the .json extension.

JSON data is normally formatted as a name:value pair, which is evaluated as a JavaScript object and follows JavaScript operations. JSON and XML are often compared, as they both carry and exchange data between various web resources. JSON is usually ranked higher than XML for its structure, which is simple, readable, self-descriptive, understandable, and easy to process.

For web applications using JavaScript, AJAX, or RESTful services, JSON is preferred over XML due to its fast and easy operation. JSON and JavaScript objects are interchangeable. JSON is not a markup language, and it doesn’t contain any tags or attributes. Instead, it is a text-only format that can be accessed through a server, as well as being able to be managed by any programming language.

JSON objects can also be expressed as arrays, dictionaries, and lists:

```
{"mymembers": [
{ "firstName":"Aasira", "lastName":"Chapagain","cityName":"Kathmandu"},
{ "firstName":"Rakshya", "lastName":"Dhungel","cityName":"New Delhi"},
{ "firstName":"Shiba", "lastName":"Paudel","cityName":"Biratnagar"},
]}
```

You have learned about JSON, which is a content holder. In the following section, we will discuss HTML styling using CSS and providing HTML tags with extra identification.

> **Important note**
> 
> JSON is also known for the mixture of dictionary and list objects it provides in Python. JSON is written as a string, and we can find plenty of websites that convert JSON strings into JSON objects, for example, `https://jsonformatter.org/`, `https://jsonlint.com/`, and `https://www.freeformatter.com/json-formatter.html`.
> 
> Please visit `http://www.json.org/`, `https://jsonlines.org/`, and `https://www.w3schools.com/js/js_json_intro.asp` for more information regarding JSON and JSON Lines.

## CSS

The web-based technologies we have introduced so far deal with content, including binding, development, and processing. CSS describes the display properties of HTML elements and the appearance of web pages. CSS is used for styling and providing the desired appearance and presentation of HTML elements.

By using CSS, developers/designers can control the layout and presentation of a web document. CSS can be applied to a distinct element in a page, or it can be embedded through a separate document. Styling details can be described using the `<style>` tag.

The `<style>` tag can contain details targeting repeated and various elements in a block. As seen in the following code, multiple `<a>` elements exist, and it also possesses the `class` and `id` global attributes:

```html
<html>
<head>
<style>
a{color:blue;}
h1{color:black; text-decoration:underline;}
#idOne{color:red;}
.classOne{color:orange;}
</style>
</head>
<body>
<h1> Welcome to Web Scraping </h1>Links:<a href="https://www.google.com"> Google </a> &nbsp;
<a class='classOne' href="https://www.yahoo.com"> Yahoo </a>
<a id='idOne' href="https://www.wikipedia.org"> Wikipedia </a>
</body>
</html>
```

Attributes that are provided with CSS properties or have been styled inside `<style>` tags in the preceding code block will result in the output shown in Figure 1.4:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_35_0.png)

Figure 1.4: Output of the HTML code using CSS

Although CSS is used to manage the appearance of HTML elements, CSS selectors (patterns used to select elements or the position of elements) often play a major role in the scraping process. We will be exploring CSS selectors in detail in Chapter 3.

> **Important note**
> 
> Please visit https://www.w3.org/Style/CSS/ and https://www.w3schools.com/css/ for more detailed information on CSS.

In this section, you were introduced to some of the technologies that can be used for web scraping. In the upcoming section, you will learn about data-finding techniques. Most of them are built with web technologies you have already been introduced to.

## Data-finding techniques used in web pages

To extract data from websites or web pages, we must identify where exactly the data is located. This is the most important step in the case of automating data collection from the web.

When we browse or request any URL in a web browser, we can see the contents as responses to us. These contents can be some dynamically added values or dynamically generated or rendered to the HTML templates by processing some API or JavaScript code. Knowing the URL of response content or finding the availability of content in some files is the first action toward scraping. Content can also be retrieved using third-party sources or sometimes even embedded in a view to end users.

In this section, we will explore a few key techniques that will help us identify, search for, and locate contents we have received via a web browser.

### HTML source page

Web browsers are used for client-server-based GUI interaction to explore web content. The browser address bar is supplied with the web address or URL, the requested URL is communicated to the server (host), and a response is received, which means it is loaded by the browser. This obtained response or page source can be further explored and searched for the desired content in raw format.
