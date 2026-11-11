The following diagram shows common node types in an a simple XML snippet:

![](bbox=[0.35, 0.17, 0.65, 0.38])

As the PHP DOM functions implement a specification based on classes, they work slightly differently than other PHP functions. The official PHP documentation takes a little bit of adjustment to find what you're used to, and because the functions are object methods, you must first have the object created in order to use them.

When you look at a DOM function in the documentation, you'll notice that the class name is also part of the function name. The classes in PHP implement required interfaces of the DOM. A class's methods are relevant to the DOM component that they are named after. For example, the DOMElement class is equivalent to the Element interface of the DOM, and affect the elements in a document. In PHP.net's function reference, other functions are usually organized alphabetically in their category. DOM XML functions in PHP.net are grouped by class before being alphabetized.

If you explore through the DOM XML documentation, you'll see that objects are very, very important. Most of the functions take in and return other DOM XML objects, and not just the simple data types of other PHP functions. Beginners will like to have the documentation open and handy to see which objects are returned with a function because the return type obviously affects which functions are available. We'll see this in action as we parse through some code.

# Parsing with DOM Functions

The first step in parsing with the DOM Functions is to load the XML document into the DOMDocument class. There is several functions you can use to do this. Each function is in DOMDocument, and are used for a specific purpose.