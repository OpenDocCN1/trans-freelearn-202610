
CSS selectors have more variety in comparison to XPath expressions. In addition, they are categorized into four basic groups, based on the complexity, code patterns, and features available. Let’s get some more information on available groups, presented in the following subsections.

## Element selectors

These are some of the basic selectors, even called common selectors, that select or choose elements from HTML. Generally, these selectors are the basic tags of HTML, as seen in the following list:

- `h1`: Selects all `<h1>` elements in the document
- `a`: Selects all `<a>` elements in the document
- `body *`: Selects group of elements inside `<body>`
- `div a`: Selects all `<a>` elements inside `<div>`
- `h1, a`: Selects all `<h1>` and `<a>` elements
- `h1 + a`: Selects element `<a>` immediately after element `<h1>`
- `h1 ~ a`: Selects every `<a>` element preceded by `<h1>`

## ID and class selectors

This type of selector adds additional features to the element selectors. In HTML where CSS has been used, we can find plenty of attributes, such as the ID represented by the # symbol and the class represented by . These are also known as global or common attributes within HTML elements. These global attributes are mostly preferred over other attributes, as they define tags for structure and identification.

Listed here are the global attributes as found in the code and expressed as follows:

- `.classOne`: Selects an individual or group of elements with `class=classOne`
- `#idOne`: Selects an element with `id=idOne`
- `a.classOne`: Selects an individual or group of `<a>` elements with `class=classOne`
- `a#idOne`: Selects an individual `<a>` element with `id=idOne`

## Attribute selectors

HTML tags contain attributes (single or multiple) that help to identify a particular element with the value it carries. These are much like the predicates used in XPath, as listed here:

- `a[href*=".org"]`: Selects `<a>` with the `href` attribute and finds `.org` in its value.
- `a[href^="https"]`: Selects `<a>` with the `href` attribute and a value that starts with `https`.
- `a[href$=".com"]`: Selects `<a>` with the `href` attribute and a value that ends with `.com`.
- `a[href~=google]`: Selects `<a>` with the `href` attribute and containing the text google. This is similar to a `[href*="google"]`.
- `[class=classOne]`: Selects elements that have `class=classOne`.
- `[href]`: Selects all elements with the `href` attribute name.

As seen in the preceding CSS selectors list, a few **regular expression (regex or Regex)** related syntaxes are also being used. This makes CSS selectors almost like regex for HTML documents. Here are some basic explanations of the symbols used in regex:

- `*` and `~`: Represent find or search, though both are used differently
- `$`: Represents the end of a string
- `^`: Represents the beginning of a string

## *Pseudo selectors*

This type of selector is a set of handy choices when it comes to identifying or selecting elements based on their positions. Here are a few with basic explanations:

- `a:eq(1)`: Selects `<a>` at position one or the first `<a>` in the document
- `a:gt(1)`: Selects all `<a>` elements located at a position greater than one
- `:not(h1)`: Selects all elements in HTML available except `<h1>`
- `a:first-child`: Preferred with nested elements, this selects every `<a>` element that is the first child of its parent
- `a:last-child`: Selects every `<a>` element that is the last child of its parent
- `a:nth-child(1)`: Selects every `<a>` element from the first child of its parent
- `a:nth-last-child(2)`: Selects every second `<a>` element from the last child of its parent
- `a:nth-of-type(2)`: Selects every second `<a>` element from its parent
- `a:nth-last-of-type(2)`: Selects every `<a>` element in the second position from the last of its parent
- `a:last-of-type`: Selects the last `<a>` element of its parent

CSS selectors are used as a convenient alternative to XPath expressions. They are somewhat shorter in length and use both simple and regex-like patterns in their expressions. CSS selectors can be converted into XPath expressions, but not vice versa.

Many open source tools and applications that deal with XPath and CSS selectors provide facilities to convert between the two, but it’s the responsibility of the developer to test such expressions and apply them accordingly in code. As we saw, in https://try.jsoup.org, such an option does exist.

https://css-selector-to-xpath.appspot.com/ is quite popular among developers, shown in Figure 3.5:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_86_0.png)

Figure 3.5: CSS selector to XPath

In this section, we explored and learned about the most popular web-related pattern-finding techniques, XPath and CSS selectors. We can use both or either one of them as per convenience, code requirements, and availability in chosen libraries. In the next section, we will explore and learn about using browser-based DevTools.

## Using web browser DevTools to access web content

DevTools are some of the most important tools available to us to explore response content of any type, such as HTML, JSON, XML, or TXT.

In the Developer tools section of Chapter 1, we introduced browser-based DevTools with various helpful, information-packed panels and a brief introduction to them. In this section, as the heading reads, we will be using DevTools to locate, find, or access the web content that we are seeking. Normally, we will search for and find the elements holding content in a similar way to how we dealt with XPath and CSS selectors using expressions.

We will explore web content using Google Chrome. Chrome has built-in DevTools with plenty of features that help us, with information on cookies, headers, curl scripts, prettifying the DOM, DOM navigation, displaying line numbers, folding/unfolding code blocks, element selection, element identification, content searching, and generating XPath and CSS selector expressions.

**Important note**
cURL, or curl, is a command-line tool and library that is used to communicate with servers and is used by machines and scripts to deal with URLs in the transfer of resources. Many programming languages use a library that communicates with curl and handles the communication with the web. For more information on curl, please visit https://curl.se/. You can also explore the Python interface to cURL (PycURL) at http://pycurl.io/.

We will now proceed with loading some URLs in Chrome and accessing some elements or navigating through the DOM using DevTools on the response content.

## HTML elements and DOM navigation

The step of accessing elements and DOM navigation is important when we are codifying the concept or planning a crawler.

At this point, we should also know to be flexible enough that whatever URL we are trying to load and process could be changed or updated. Therefore, to deal with such situations occurring, we have selected a few sites that are available online for people to learn about web scraping and implement scraping. We also used those sites in the first edition of this book, Hands-On Web Scraping with Python by Packt Publishing. You are free to use a similar concept and steps with any other URL.

We will be using https://toscrape.com. This site provides two different URLs, http://books.toscrape.com and http://quotes.toscrape.com/, both loaded with plenty of categories, labeling, paginated content, DOM elements, authorization-related sections, and much more. Let us load http://books.toscrape.com using Google Chrome, as seen in Figure 3.6:

![](img/61ad17ff4226a1bcbcd5f7a33a05dc4f_87_0.png)

Figure 3.6: Loading books.toscrape.com

As the page content is successfully loaded, we can load DevTools with a right-click on the page and by selecting the Inspect option or by pressing Ctrl + Shift + I. With DevTools available, first, we can find a pointer icon listed on the left. This is used for selecting elements from the page, as shown in Figure 3.7. This element selector (also known as the element inspector) located in the top-left corner of DevTools, can be turned on and off by clicking it or pressing Ctrl + Shift + C:
