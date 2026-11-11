
The next three statements use XMLHttpRequest methods and properties. onreadystatechange is a property that holds the JavaScript callback function for this object. Set this to the name of the function, without opening and closing parentheses, that will be executed when the server responds. You can only select one callback function. To execute more, you will need to create a facade wrapper function that executes the others, and set the facade function as the callback.

open gets the object ready to send the request. The first two parameters are required. The first parameter is the HTTP method to use. The second is the URL. The third parameter is whether the object should be in asynchronous mode. It is optional, but it is a good idea to set this to true because the default value is false, and we do want to be in asynchronous mode. Otherwise, we would be in synchronous mode, which means that the rest of the JavaScript does not execute until XMLHttpRequest receives a response from the server.

send actually sends the request. send takes one required parameter, the body of the request. In this example, we are sending a null because we are just doing a GET request. The request does not have a body. If we were doing a POST, we would construct the parameters in a separate string and pass it as send's parameter. After send is called, the HTTP request is made and the callback function executes.

## Creating and Using the Callback

There are two main jobs of the callback function. The first is to capture the server response. The second is to do something with that response.

We start off our function with a couple of checks to make sure the data from the server has indeed arrived. If we didn't do this, the rest of our code will execute prematurely and without all the necessary parts from the server response.

The first if statement checks the readyState property of the XMLHttpRequest object. As the request executes and processes, this value gets changed. There are five possible values of this property:

| readyState value | Meaning |
|---|---|
| 0 | Uninitialized |
| 1 | Loading |
| 2 | Loaded |
| 3 | Interactive |
| 4 | Completed |

Only when the value is 4 is the data completely ready to be parsed and used by the web application.

The second if statement checks to see XMLHttpRequest's status property. This is the same code that reports 404 for missing file, 500 for internal server error, etc. A 200 is a successful transaction. We need to make sure the request is executed successfully, or the data might be useless.

```
function parseResponse() {
    if (g_xmlHttp.readyState == 4) {
        if (g_xmlHttp.status == 200) {
            var response = g_xmlHttp.responseXML;
            var outputArea = document.getElementById("ServerResponse").
                firstChild;
            var responseElements =
                response.getElementsByTagName("textField");
            outputArea.nodeValue =
                responseElements[0].firstChild.nodeValue;
        }
    }
}
```

The first line after the nested if statement captures the value in the responseXML property of the XMLHttpRequest object and places it in a variable. This property is where the browser keeps the response from the server. If you were to inspect it, you would see the direct XML from the server.

The second statement captures the node of the HTML page of where we are going to output the response. We use JavaScript's getElementById() function and traverse down the DOM.

We can use the same DOM functions in JavaScript to extract the information from the server response. This is what we do in the third statement. We know what we are interested in is located in the textField element of the response. We zero in on that and get that node.

Each DOM element keeps the text it displays in a property called nodeValue. In the fourth statement, we set the output area's nodeValue to the nodeValue of the response. This changes the webpage every time it is executed.

If you type in the text field of `ajaxTest.php`, you can see this code in action.

![](img/c637937d44400f1da21fb7cca101de80_246_0.png)

> In our code, we checked for an HTTP status of 200. While this is good practice, it requires the HTTP network protocol to be present in order to work. This means you must load the page in a web browser through HTTP. If you load the page through the file system (i.e. through `file:///ajaxTest.php`, instead of `http://localhost/.../ajaxTest.php`), status check will fail, and the code will not execute properly.

This is the standard way of triggering an AJAX application, and it works very nicely. The DOM parsing, however, can get messy. There are two DOMs you must parse—the local web page and the server response. Fortunately, you may have some alternatives to parsing the server response.

First off, `responseXML` has a sister property, `responseText`, that works exactly the same way. `responseText` holds the server response if it is any text string instead of XML. You can immediately use the response text instead of traversing through a DOM to get what you want. If you are merely a front-end developer for a much larger web development, and the company manifesto is to transfer everything via XML, this might not be an available option for you. Or, if your web service is used by third parties, it may be best to keep it as XML. However, if you are writing a very simple service to support just your application, know that you do not have to structure everything in XML. You can just pass a simple text string back and use `responseText` on the client end instead.

If your web service response is too complicated for a simple text string, you may want to consider formatting your text response in JavaScript Object Notation (JSON) to send this result back to the page. It will still be a text response, so you can use `responseText` and skip the parsing. JSON gives you the structure of XML with the simplicity of a text string. This next section will introduce us to JSON.

## Debugging AJAX

![](img/c637937d44400f1da21fb7cca101de80_247_0.png)

Debugging the request and response from the server can be tricky. We can't use a regular IDE. We need something to watch the HTTP streams. Luckily, if you are using Firefox, there is a Greasemonkey script that will do just that. Greasemonkey is a Firefox extension that allows users to write their own JavaScript and code against a site when they visit it. It can be found at https://addons.mozilla.org/firefox/748/. Once you have that installed, download the XMLHttpRequest debugging tip at http://blog.monstuff.com/archives/000250.html. This tool will watch everything that comes out from the browser, and everything going in. Other helpful extensions for Firefox include LiveHTTPHeaders, which show the request and response HTTP headers, and Firebug, a general JavaScript and CSS debugger. For Internet Explorer, a commercial tool called HTTPWatch is available to watch HTTP requests.

## JavaScript Object Notation (JSON)

JavaScript Object Notation is simply a transfer format, much like SOAP or XML-RPC. Unlike those two formats, JSON is not XML based. It is JavaScript code that is loosely based on a C-style definitions and formats. Although called JavaScript Object Notation, many server side languages have built parsers to interpret JSON format. Given this and its lightweight nature, it has become a popular alternative to XML when communicating between a web browser and a client. JSON's home page is at http://www.json.org.

## JavaScript Objects Review

Let's quickly review JavaScript objects first. To define a class in JavaScript, you simply treat it as if it was a function. To give the class properties, use the keyword this, followed by a dot, followed by the name of the property. To give the class methods, also use this, followed by a dot, the name of the function, an equal sign, the keyword function and then the function definition. For example, this could be a cat object in JavaScript:

```
function Cat (name) {
    this.name = name;
    this.gender;
    this.age;
    this.eat = function() {
        alert("Yum");
    }
    this.sleep = function() {
```
