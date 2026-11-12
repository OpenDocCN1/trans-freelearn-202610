The extent function in the preceding code returns the array called [min, max] with the minimal and maximal x value of the dataset.

To extract other statistical information from the dataset, we can use the following:

+   • d3.mean(arr, accessor),
+   • d3.median(arr, accessor),
+   • d3.variance(arr, accessor)
+   • d3.deviation(arr, accessor)

> The filter, map, and reduce functions have their origin in functional programming as they work on an input array, transform it independently of a state or context, and output a new array at the end. A benefit of using a reduce function over a simple loop is that you execute your code inside a local scope, whereas in a loop you would be still in the same scope and maintain a state.
> Talking about performances, the filter, map, and reduce functions are rarely faster than the alternative with a custom loop. This mainly depends on the JavaScript engine and how it optimizes these functions. However, as I said before, they allow the developer to write more beautiful and cleaner code, as they work in a separated scope independently of a state. If this interests you, you should definitely check out more about Functional Programming!

# # Loading and parsing remote data

In this section, we want to visualize real data rather as in sample dataset; hence, we need to understand how to load data from remote locations. In JavaScript, you can use the native XMLHttpRequest (XHR) object to fetch data within your application. D3 implements not only a wrapper for the native XHR call, but it also implements wrappers for loading different data types and parsing them automatically.

Here is a list of some high-level wrappers in D3:

+   • d3.text(url[, mimeType[, callback]]): This loads a plain text file with a mimeType from an URL; the data is handed over as a string to the callback where the parsing must be done manually
+   • d3.json(url[, callback]): This loads and parses a JSON file from an URL; the data is passed as an object in the callback
+   • d3.xml(url[, mimeType[, callback]]): Loads and parses an XML file from an URL, the data is passed as an object in the callback