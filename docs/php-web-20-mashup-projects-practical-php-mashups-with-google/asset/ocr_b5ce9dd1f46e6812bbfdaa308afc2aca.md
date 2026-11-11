The next three statements use XMLHttpRequest methods and properties. onreadystatechange is a property that holds the JavaScript callback function for this object. Set this to the name of the function, without opening and closing parentheses, that will be executed when the server responds. You can only select one callback function. To execute more, you will need to create a facade wrapper function that executes the others, and set the facade function as the callback.

open gets the object ready to send the request. The first two parameters are required. The first parameter is the HTTP method to use. The second is the URL. The third parameter is whether the object should be in asynchronous mode. It is optional, but it is a good idea to set this to true because the default value is false, and we do want to be in asynchronous mode. Otherwise, we would be in synchronous mode, which means that the rest of the JavaScript does not execute until XMLHttpRequest receives a response from the server.

send actually sends the request. send takes one required parameter, the body of the request. In this example, we are sending a null because we are just doing a GET request. The request does not have a body. If we were doing a POST, we would construct the parameters in a separate string and pass it as send's parameter. After send is called, the HTTP request is made and the callback function executes.

# Creating and Using the Callback

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