
```xml
<?xml version="1.0" encoding="UTF-8"?>
<breakfast_menu>
    <food>
        <name>Brown Toast-Honey</name>
        <price>$2.99</price>
        <description>All time famous, honey and toast combination.</description>
    </food>
    <food>
        <name>Oat-Pancake</name>
        <price>$4.95</price>
        <description>Delicious pancake, with cheese and berry on top.</description>
    </food>
    <food>
        <name>Creamy Yoghurt</name>
        <price>$3.99</price>
        <description>Dried nut filled yoghurt, thick and creamy.</description>
    </food>
</breakfast_menu>
```

As seen in *Figure 3.2*, we can deduce the following:

- Everything is a node in XML – `breakfast_menu`, `food`, `name`, and more
- An XML node can be an element itself, having start and end tags
- The `breakfast_menu` parent node has three child nodes called `food`
- Each food node has its own child nodes: `name`, `price`, and `description`
- There is no name-value pair or attributes in any nodes

For an XML document such as the one in *Figure 3.2*, most text or code editors provide some formatting options, for example, folding or unfolding an element or parent node and beautifying the text, XPath query-related features, and more.

> **Important note**
> 
> Numerous platforms and applications available online or offline provide XML formatting (plus code-related features), beautifying options, and XPath expression testing. A few such online providers are https://codebeautify.org, https://www.freeformatter.com, https://xpathtester.com, https://try.jsoup.org/, and https://www.webtoolkitonline.com/, among others.

In the following example, we will be using XPath Tester from Code Beautify (https://codebeautify.org/Xpath-Tester) to format XML content and test XPath expressions:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_80_0.png)

Figure 3.3: XPath Expression – //food[price>1.99]/name

As seen in Figure 3.3, the //food[price>1.99]/name expression is applied to the XML file in Figure 3.2.

As seen in the following output, the XPath expression applied in Figure 3.3 returns only the name element of those food nodes where the predicate defined as [price>1.99] matches the price value greater than 1.99:

```
<name>Brown Toast-Honey</name>
<name>Oat-Pancake</name>
<name>Creamy Yoghurt</name>
```

A predicate is used to identify a specific node or element. They are written using square brackets, which are similar in syntax to Python lists. Here are a few XPath expressions with general explanations applied on the breakfast.xml file seen in Figure 3.2:

- // : Document node
- *: All elements in the document
- //breakfast_menu: Root element and all elements within breakfast_menu
- //food: Selects all food elements
- //food/name: Selects the name element from all food elements
- //food[1]/name/text(): Returns only the text from name of the first food element
- //food[1]/name: Selects the name of the first food element, for example, <name>Brown Toast-Honey</name>
- //food[2]/*: Selects all elements from food in the second position

- `//food[position() <3]/price`: Selects the price of the food elements that are located in a position less than 3
- `//food[price>3.99]/name`: Filters and selects all name elements from food elements where price has a value greater than 3.99, for example, <name>Oat-Pancake</name>
- `//food[last()]/name`: Selects the name element of the last food element
- `//food[last()]/name | //food[last()]/price`: Selects the name and price elements from the last food element
- `sum(//food/price)`: Sums all the values of calories inside food, which adds up to the value 11.9
- `//food/name[contains(.,"Toast")]`: Selects the name element of the food element that contains the "Toast" string
- `//food/description[starts-with(.,"Dried")]`: Selects the description element from the food element that matches the option that starts with the "Dried" string
- `//food[price>1.99 and price<4.00]/name`: Selects the name element from food elements where the price>1.99 and price<4.00 filter options match

Now, with a basic idea about XPath expressions, let us consider some expressions with attributes. Attributes are extra properties that identify certain parameters for a given node or element. A single element can contain some unique or common attributes. Attributes in HTML play a significant role in efficient and effective traversing. Let us consider the https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter03/books.xml XML file with the following content:

```
<books>
    <book id="1491946008" price="47.49">
        <author>Luciano Ramalho</author>
        <title>Fluent Python: Clear, Concise, and Effective Programming</title>
    </book>
    <book id="1491939362" price="29.83">
        <author>Allen B. Downey</author>
        <title>Think Python: How to Think a Computer Scientist</title>
    </book>
</books>
```

XPath expressions accept key attributes by adding or prepending the @ character to the key. Listed here are a few expressions using attributes with brief explanations:

- `//book/@price`: Selects the price attribute of all books available

- `//book[@price>30]`: Selects all elements of book where the price attribute is greater than 30
- `//book[@price<30]/title`: Selects all the title elements from book where price is less than 30
- `//book/@id`: Selects the book attribute of id and its value
- `//@id`: Selects the id attribute and its value
- `//book[@price="29.83"]/author`: Selects author from book where the price attribute matches 29.83
- `sum(//@price)`: Returns the total of price attribute values, i.e., 77.3

In this section, we have looked at a few examples and expressions to learn about XPath and how can we use such expressions to retrieve the desired content. Similar to XPath, we also have selectors based on CSS, which we will explore in more detail in the next section.

## CSS selectors

In the *Understanding the latest web technologies* and *Data-finding techniques used in web pages* sections in *Chapter 1*, we learned about CSS and its use to style HTML. There are various ways to apply CSS to HTML.

CSS selectors (CSS queries or CSS selector queries) are defined patterns used by CSS to select elements. Similar to XPath expressions, which are used to find and identify elements, CSS selectors are used to select or find HTML elements and define a style for them. In the extraction process, we search for and find elements using CSS selectors.

The following code shows plain HTML with basic tags:

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
    <h1> Welcome to Web Scraping </h1> Links:
    <a href="https://www.google.com"> Google </a> &nbsp;
    <a class='classOne' href="https://www.yahoo.com"> Yahoo
    </a> &nbsp;
    <a id='idOne' href="https://www.wikipedia.org">
```

```
Wikipedia </a>
</body>
</html>
```

As seen in the preceding code, we have basic HTML defined in the https://github.com/PacktPublishing/Hands-On-Web-Scraping-with-Python-Second-Edition/blob/main/Chapter03/css_selector.html file, we can collect information such as the following:

- <h1> is an HTML tag, an element, and a selector
- The first selector, <a>, has a single href attribute and it contains the text Google
- The second <a> selector has multiple attributes, such as class with the value classOne
- The last <a> selector also has multiple id attributes with the value idOne, href with some values, and the text Wikipedia

The distinguished selectors, with or without attributes, in the preceding source code can be used to select that particular element individually or in a group. Similar to XPath Tester, there are plenty of DOM parser providers available online and offline with CSS query-related facilities. We will be using https://try.jsoup.org, as seen in Figure 3.4:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_83_0.png)

Figure 3.4: Evaluating CSS query a#idOne
