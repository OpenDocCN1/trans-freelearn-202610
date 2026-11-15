
# HTML Fundamentals

## Core Concepts

### id and class Attributes

Two attributes appear constantly in HTML: `id` and `class`. An `id` identifies one unique element on a page. A `class` labels one or more elements as belonging to the same group. Later, CSS and JavaScript will use these heavily. For now, the important `id` idea is simple: `id` is for one specific element, while `class` can be shared.

### Void and Container Elements

Some elements are **void elements**, like `<img>` or `<br>`, which do not have closing tags or inner content. Others are **container elements**, such as `<div>` or `<section>`, which wrap content and can include other elements inside them.

### The div Element

A `<div>` is a generic container element with no inherent meaning. It’s useful for grouping content for styling, but it should not replace semantic elements like `<section>`, `<nav>`, or `<p>` when those are more appropriate.

### Element Nesting

It is also important to know that elements can be nested, meaning one element can sit inside another to create a hierarchy. Proper nesting is important because incorrect structure can lead to unexpected rendering or accessibility issues.

### Accessibility

**Good practice**: Accessibility in web development means designing and structuring content so that it can be used by everyone, including people with disabilities such as visual, hearing, motor, or cognitive impairments. This includes using proper HTML structure, meaningful text alternatives like `alt` attributes for images, and clear navigation so assistive technologies such as screen readers can interpret the page correctly.

### Block-level vs Inline Elements

Another key idea is the distinction between **block-level elements** and **inline elements**. Block-level elements, like `<p>` and `<h1>`, typically start on a new line and take up the full width available, while inline elements, like `<span>` or `<a>`, flow within text without breaking the layout.

### Semantic HTML

Modern HTML emphasizes semantic elements, such as `<article>`, `<nav>`, and `<footer>`, which describe the meaning of content rather than just its appearance. Using these correctly improves accessibility, search engine understanding, and overall code clarity.

You will reuse these ideas throughout the chapter.

> **Prompt**: Please explain the difference between semantic HTML and non-semantic HTML with real-world examples and why it matters for accessibility.

## HTML Comments

HTML comments let you leave notes in the source code that are not shown on the page. They look like this:

```
<!-- This is a comment -->
```

Comments are useful for reminders, temporary notes, or explaining examples in teaching code. They are not a way to hide sensitive information, because anyone can still view the page source.

## The Basic Structure of a Web Page

Every HTML page follows the same overall pattern:

```
<!DOCTYPE html>
<html>
  <head>
    <title>My first page</title>
  </head>
  <body>
    <h1>Hello world</h1>
    <p>This is my first web page.</p>
  </body>
</html>
```

Each part has a distinct role:

* `<!DOCTYPE html>` tells the browser *this is modern HTML*, meaning standards-based HTML, specifically HTML5. If you omit it, browsers may fall back to **quirks mode**, which emulates legacy behavior from the early web to maintain compatibility with very old pages. That can lead to inconsistent layout, especially with CSS.
* `<html>` wraps the entire document.
* `<head>` contains metadata like the title, character encoding, and links to CSS for applying styles.
* `<body>` contains everything the user can see.

Nothing visible on the page goes outside `<body>`. The page title is defined in `<head>` and usually appears in the browser tab.

> **Prompt**: Please show me examples of good and bad HTML structure and explain what makes one better than the other.

## Try It Out!

Use your favorite text editor to create a file named `myfirstpage.html`. Copy the previous HTML into the file and save it. Then open the file in your preferred web browser. Compare the file in your text editor and browser side-by-side, as shown in *Figure 2.1*:

![](Images/B34553_02_01.png)

Figure 2.1: Comparing the source code for a web page to its visual look using Notepad and Microsoft Edge on Windows

Instead of a plain text editor and a web browser, you can also try out your work using VS Code and its Live Preview extension, as shown in *Figure 2.2* and *Figure 2.3*:

![](Images/B34553_02_02.png)

Figure 2.2: Click the Show Preview button to see a Live Preview of a web page

![](Images/B34553_02_03.png)

Figure 2.3: Comparing the source code for a web page to its visual look using VS Code

For the rest of this book, I recommend using VS Code with Live Preview, since it gives immediate feedback as you work and matches the workflow used in later chapters.

> **Prompt**: Please show me the HTML for a basic web page with examples of headings, paragraphs, and lists, so that I can learn about how web pages are structured, with an explanation of all the elements.

## Headings Give Content Hierarchy

Headings define document structure. Their visual size is a styling decision. The font family and font size for each heading level will be applied using CSS later and do not have to match the level number!

HTML has six heading levels:

```
<h1>Main page title</h1>
<h2>Section title</h2>
<h3>Subsection title</h3>
<h4>Sub-subsection title</h4>
<h5>Sub-sub-subsection title</h5>
<h6>Sub-sub-sub-subsection title</h6>
```

Key beginner rule:

* `<h1>` is the most important heading on the page
* `<h2>` is a section inside that page
* `<h3>` is a subsection inside an `<h2>`

Treat headings as part of the page outline, not as a way to pick a bigger font.

Search engines, screen readers, and accessibility tools rely heavily on correct heading structure. Using headings purely for visual effect is one of the most common beginner mistakes.

## Paragraphs Hold Readable Text

Normal text belongs in paragraphs:

```
<p>This is a paragraph of text.</p>
```

Browsers automatically add spacing between paragraphs.

You do not press *Enter* repeatedly like in Word. In fact, extra whitespace characters like carriage returns, tabs, and spaces are ignored when rendering HTML in a browser!

HTML is about meaning, not layout.

## Links Connect the Web

Links are what connect pages into the web:

```
<a href="https://example.com">Visit Example Website</a>
```

Here:

* `<a>` stands for *anchor*
* `href` is the destination
* The text between the tags is what users click on

Links can point to:

* Other websites
* Other pages on the same site
* Sections within the same page

Without links, HTML pages are just isolated documents.

## Images Add Visual Content

Images are embedded using the `<img>` element:

```
<img src="photo.jpg" alt="A photo of a cat">
```

Note the following about the preceding code:

* `<img>` has no closing tag
* `src` is the image file location
* `alt` describes the image in words

`<img>` is a void element, which means it has no closing tag because it doesn’t wrap any other content or contain nested elements. Other void elements you’ll encounter include `<br>` (line break), `<hr>` (horizontal rule), and `<input>` for forms. While `<img>` does not need a closing tag, it does need certain attributes to be considered well-formed and accessible, like `src` and `alt`.

The `alt` text is important because it carries meaning when the image cannot. Screen readers rely on it. Search engines read it. If the image fails to load, the text still communicates meaning.
