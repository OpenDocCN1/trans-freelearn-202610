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