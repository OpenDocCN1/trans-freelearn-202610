XML is an open standard, using the Unicode character set. XML is used to share data across various platforms and has been adopted by various web applications. Many websites use XML data, implementing its contents with the use of scripting languages and presenting it in HTML or other document formats for the end user to view.

Extraction tasks from XML documents can also be performed to obtain the contents in the desired format, or by filtering the requirement with respect to a specific need for data. Plus, behind-the-scenes data may also be obtained from certain websites only.

> **Important note**
> 
> Please visit `https://www.w3.org/XML/` and `https://www.w3schools.com/xml/` for more information on XML.

So far, we have explored content placing and content holding related technologies based on markup languages such as HTML and XML. These technologies are somewhat static in nature. The next section is about JavaScript, which provides dynamism to the web with the help of scripts.

# JavaScript

**JavaScript** (also known as **JS** or **JScript**) is a programming language used to program HTML and web applications that run in the browser. JavaScript is mostly preferred for adding dynamic features and providing user-based interaction inside web pages. JavaScript, HTML, and CSS are among the most-used web technologies, and now they are also used with headless browsers (you can read more about headless browsers at `https://oxylabs.io/blog/what-is-headless-browser`). The client-side availability of the JavaScript engine has also strengthened its usage in application testing and debugging.

`<script>` contains programming logic with JavaScript variables, operators, functions, arrays, loops, conditions, and events, targeting the HTML **Document Object Model (DOM)**. JavaScript code can be added to HTML using `<script>`, as seen in the following code, or can also be embedded as a file:

```
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
```