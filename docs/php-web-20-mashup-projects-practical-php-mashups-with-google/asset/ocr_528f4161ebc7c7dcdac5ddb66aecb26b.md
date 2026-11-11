Our start element callback simply displays the element name and loops through any attributes the element has.

```
function startElement($parser, $name, $attributes) {
    echo "Start: " . $name . "<br />";
    foreach ($attributes as $key => $value) {
        echo "Attribute: " . $key . " :: " . $value . "<br />";
    }
}
```

PHP passes the parser, the name of the element, and the attributes as an array to the start element callback. If the element has any attributes, they are passed to the callback as an associative array in the third parameter. Here, we simply loop through the array.

The end element callback has two parameters passed to it—the parser and, again, the name of the element.

```
function endElement($parser, $name) {
    echo "End:" . $name . "<br>";
}
```

Our last callback function is the function that's called whenever the parser encounters character data, that is, data that is not an element.

```
function characterData($parser, $data) {
    echo "Data:" . $data . "<br>";
}
```

The parser is passed as the first parameter and the value of the data is passed as the second. Again we simply echo out the data.

# Seeing the Callbacks in Action

Now it's time to see the script in action and see how the callback functions are being called. Save the script in an area servable by your web server. Hit the script with a web browser.