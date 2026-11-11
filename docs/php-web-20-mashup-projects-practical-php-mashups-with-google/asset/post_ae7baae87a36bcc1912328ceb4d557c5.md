
The majority of the DOM classes are interfaces for the Node class. This means that if you have an object that extends Node, you will have access to these navigation properties. This not only gives you the ability to navigate through the document easily, but it gives you options on how you can extract data.

For example, DOMElement extends DOMNode. If you have a DOMElement object, you can grab all of the child nodes back as a DOMNodeList simply by using DOMNode's childNodes property. DOMNode's traversal properties is summarized in the following table:

| Property Name | Return Type | Description |
|---|---|---|
| parentNode | DOMNode | The parent node that contains the DOMNode |
| childNodes | DOMNodeList | A DOMNodeList of all direct child nodes of this DOMNode |
| firstChild | DOMNode | The first child node of the DOMNode |
| lastChild | DOMNode | The last child node of the DOMNode |
| previousSibling | DOMNode | The node immediately preceding the DOMNode |
| nextSibling | DOMNode | The node immediately following the DOMNode |

One of the things we did in our previous example was to grab the dates only associated with a post. We grabbed the posts, looped through it, extracted another NodeList using getElementsByTagName, now use the item method and pass it the assumed index position.

```
<?php
$posts = $aDom->getElementsByTagName('post');
foreach ($posts as $post) {
    $postDate = $post->getElementsByTagName('date'); ?>
    Date: <?= $postDate->item(0)->nodeValue ?><br />
<?php } ?>
```

We could have also written it this way:

```
<?php
$posts = $aDom->getElementsByTagName('post');
foreach ($posts as $post) {
    $childNodes = $post->childNodes; ?>
    Date: <?= $childNodes->item(1)->nodeValue ?><br />
<?php } ?>
```

In this example, we grab the NodeList of posts again and loop through it. This time, however, we don't have to call getElementsByTagName a second time. Instead, we grab the child nodes using DOMNodes's childNodes property. This also returns a NodeList, so we can use the item method to grab the first element. The main advantage to this method is that we are using a property (childNodes) instead of a function call (getElementsByTagName) to get the same information. Accessing properties is theoretically faster than calling a function because there is no thinking necessary from the parser — the data is just passed and assigned.

## Beware of White Space

![](img/c637937d44400f1da21fb7cca101de80_190_0.png)

We use an index position of 1 for the item method because in the XML created, line breaks after the post element. The first item returned, index(0) is the line break itself. It is DOMText object containing white space. What we really want is the second item, which is the date element. This is a common pitfall when working with all XML documents. Even though you may see an element right before or after the element you're interested in, there may be white space character data objects in-between.

Using a full HTML file, let's take a look at another example on using these properties. The example file named examplehtml.html is a small, valid HTML file with two lists.

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
"http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
    <title>Test ID Page</title>
</head>
<body>
    <p>This is a test of lists.</p>
    <ul id="myList">
        <li>First Item</li>
        <li>Second Item</li>
        <li>Third Item</li>
    </ul>
    <ul id="anotherList">
        <li>You</li>
        <li>Won't</li>
        <li>See</li>
        <li>This</li>
        <li>At</li>
        <li>First</li>
    </ul>
</body>
</html>
```

Note the first unordered list has an id attribute of myList, and the second unordered list has an id attribute of anotherList. The file domxmlexample2.php is a PHP script that does some basic extraction from this HTML file.

```
<?php
    $aDom = new DOMDocument();
    try {
        $aDom->loadHTMLFile('examplehtml.html');
    } catch (Exception $ex) {
        $aDom = false;
    }
}
```

The beginning of this script does the same thing as the first example. We create a new DOMDocument object and load some XML into it. This time, we use loadHTMLFile() to load an HTML file.

```
$firstUL = $aDom->getElementById('myList');
$secondUL = $firstUL->nextSibling;
?>
```

In this next section of code, we grab the two unordered list nodes. The first list is retrieved using the getElementById() method which takes the value of an id attribute as the parameter and returns a DOMElement. The second unordered list could be extracted using the same method. However, as we know the second list is on the same DOM tree level as the first list, and it also immediately follows the first list, we can use DOMNode's nextSibling property to retrieve it.

```
Looping through the first list:<br />
<?php foreach ($firstUL->childNodes as $value) { ?>
    <?= $value->nodeValue ?><br />
<?php } ?>
```

This foreach loop uses the childNodes property, which is a DOMNodeList, to retrieve the items in the first unordered list. Each DOMNode in the loop gets assigned to $value. The nodeValue property in each DOMNode is the value of each li element. Echoing out the nodeValue property will display the li value on the screen.

```
Looping through the second list:
<?php foreach ($secondUL->childNodes as $value) { ?>
    <?= $value->nodeValue ?><br />
<?php } ?>
```

The second list's ul element was extracted using the nextSibling property. This property is a DOMNode. This is different from $firstUL because getElementById() returns a DOMElement. Nevertheless, for our purposes, this is good enough because the core navigation properties and values are in the DOMNode object. Like the previous foreach loop, this block gets the child nodes of the element back, and those nodeValue properties are used to display the value of the li element.

```
What's the first item? <b><?= $firstUL->firstChild->nodeValue ?></b>
```

Finally, this last line gets the first item of the first unordered list and displays it. $firstUL is the first unordered list element. firstChild property holds the li element. Finally, we display its nodeValue property to get the information about the list. The purpose of the last line is to show how to access elements far away from your current element. Properties, because they are objects, can be chained. It is perfectly reasonable to traverse the DOM tree like such:

```
$testNode->parentNode->previousSibling->firstChild
```

Although wordy and long, this would go up one level from the node you are in, across one level to the previous node on the same level, and then down one level. If you think of this in familial relationships, if you are the testNode, then your parent's sibling's child would be your cousin.

Running this example script in the browser will show how this parsing results:

![](img/c637937d44400f1da21fb7cca101de80_192_0.png)

The DOM section in the official PHP documentation lists the implemented DOM classes, their methods, and their properties. Sometimes, though, it's easier to see things graphically. The following diagram shows the PHP implemented DOM classes as a UML class diagram.

![](img/c637937d44400f1da21fb7cca101de80_193_0.png)

This diagram shows the inheritance chain of each object, and every project's properties and methods. At a glance, we can see which class can use the DOMNode properties and methods thanks to inheritance as well as the data types we are dealing with. Class diagrams are not that hard to interpret. Each box is a class. A class is divided up into three sections. The top section is the name of the class. The middle section lists the properties. The bottom section lists the methods. If the middle or bottom section is empty, the class does not have any properties or classes, respectively.

Properties are listed in the following format:

```
VisibilityIndicator PropertyName:DataType
```

Visibility indicators are a plus sign (+) for a public property, hash (#) for protected, and a minus sign (-) for private. In the case of PHP DOM, all properties are public. If a property has [READONLY] after the property name, it means that the property cannot be changed by the developer. In DOMNode, there is a property listed as +ownerDocument [READONLY] :DOMDocument. This means that there is a public property named ownerDocument. It is read only and accessing it gives you a DOMDocument data type.

Methods are listed in this format:

```
VisibilityIndicator MethodName(parameters):returnType
```

The visibility indicators are the same as the properties, and all are public in PHP DOM. The method name is self-explanatory. The parameters are in a parameterName:datatype pair. You shouldn't worry about the parameter name because it is used by the function. It is just listed to give you a hint on what to pass. If a parameter is not in brackets ([]) then it is required. Parameters in brackets are optional. Finally, the returned value's data type is given. Let's look at a method in DOMDocument: +loadXML(source:Str[,options:Int]):Mixed. This is a public method named loadXML. It has one required string parameter named source, and an optional one named options which is an integer. It returns a mixed data type. You'll have to consult the documentation to find out exactly what is the source and options, and what type of "mixed" data you get back.

An arrow from one class to another shows inheritance, with the parent being the class that is being pointed. DOMText's parent is DOMCharacterData. DOMCharacterData's parent is DOMNode.
