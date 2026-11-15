
# References and resources

Let's review the official HTML documentation and references.

## MDN web docs (beginners should start here)

MDN Web Docs is the best HTML documentation on the internet for learning and day-to-day reference. It is maintained by Mozilla with contributions from browser vendors and the web community.

* Homepage: [https://developer.mozilla.org/](https://developer.mozilla.org/)
* HTML overview: [https://developer.mozilla.org/en-US/docs/Web/HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)
* HTML elements reference: [https://developer.mozilla.org/en-US/docs/Web/HTML/Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element)
* Forms and input elements: [https://developer.mozilla.org/en-US/docs/Learn/Forms](https://developer.mozilla.org/en-US/docs/Learn/Forms), [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form), and [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)

MDN is so good because it is written for humans, not lawyers, with uncomplicated examples, and it explains why, not just what. It is kept in sync with real browser behavior.

**Good practice**: If a beginner uses only one documentation site while learning HTML, it should be MDN.

## W3C HTML specification (official but not beginner friendly)

The **World Wide Web Consortium** (**W3C**) publishes the official HTML specifications. This is the formal definition of how HTML works at a standards level.

* HTML specification landing page: [https://www.w3.org/TR/html/](https://www.w3.org/TR/html/)
* Living HTML standard (WHATWG, see below): [https://html.spec.whatwg.org/](https://html.spec.whatwg.org/)

The specification is written for browser implementers like the programmers at Microsoft, Google, and Apple, who build rendering engines. It is not written to teach HTML. It is dense, precise, and unforgiving.

This is reference material, not learning material. You don't read it cover to cover. You look things up when you need exact behavior.

## WHATWG HTML living standard

Most browsers now follow the WHATWG HTML Living Standard, which evolves continuously instead of being frozen into versions.

HTML Living Standard: [https://html.spec.whatwg.org/](https://html.spec.whatwg.org/)

For current browser behavior, this is the main specification to check.

Again, this is not beginner reading. It's for checking edge cases, resolving ambiguity, and understanding how browsers really behave.

## How beginners should actually use these resources

A practical approach is to start with MDN, build a small example, and consult the specification only when needed:

* Concepts
* Examples
* Best practices

Then get your hands dirty by building something:

* Make mistakes
* Break things
* Fix them

Only then glance at the official specifications if:

* Browser behavior seems odd
* You need absolute precision
* You're curious how the machinery works

For learning HTML, use MDN Web Docs. It explains HTML the way humans think.

The W3C and WHATWG specifications define how browsers work, but they are not teaching material. Treat them as reference manuals, not tutorials.

# Summary

In this chapter, you learned the following:

* HTML describes the structure and meaning of content, not how it looks
* An HTML document is built from elements, which are made of tags and content
* Attributes provide additional information that configures how elements behave
* Headings create a logical outline of a page and should reflect meaning, not appearance
* Paragraphs, lists, links, images, and tables represent different types of content
* Elements can be nested to form a hierarchical structure that browsers interpret as a tree
* Block-level elements structure layout, while inline elements flow within text
* Semantic HTML improves accessibility, search engine understanding, and maintainability
* Forms allow users to input data and submit it to a server using `GET` or `POST`
* Every form control must have a name to be included in submitted data
* HTML is forgiving and designed for documents, unlike stricter markup languages like XML
* SVG uses markup to describe scalable graphics and can be embedded in HTML
* Good HTML creates a strong starting point for CSS styling and JavaScript interactivity

In the next chapter, you will learn about CSS, a language used to provide styling and layout to the content on a web page.

# Get this book's PDF version and more

Scan the QR code (or go to packtpub.com/unlock). Search for this book by name, confirm the edition, and then follow the steps on the page.

![](Images/Image16900.png)![](Images/Image16908.png)

*Note: Keep your invoice handy. Purchases made directly from Packt don't require an invoice.*

# 3

# CSS – Styling Your Web Pages

Make your pages beautiful using `Cascading Style Sheets (CSS)`. This chapter shows how CSS controls presentation and layout.

By the end of this chapter, you will have taken a plain HTML document and transformed it into a visually appealing, readable, and responsive web page. You will understand how styles are applied, how to control color and typography, how elements are positioned on the page, and how websites adapt to different screen sizes. You will also learn how to spot and fix common CSS mistakes, including using AI tools to help debug layout and styling problems when things do not look the way you expect.

In this chapter, we will cover the following topics:

* CSS basics
* Colors, fonts, and layout
* Responsive design
* AI challenge – CSS debugging
* Transforms and animation

Suggested chatbot prompts with example responses for this chapter can be found at the following link: [https://github.com/markjprice/web-dev-ai/blob/main/prompts/ch03-css/readme.md](https://github.com/markjprice/web-dev-ai/blob/main/prompts/ch03-css/readme.md)

## CSS basics

CSS exists to control presentation: HTML describes what content is, CSS describes how that content should look. Keeping those concerns separate makes web pages easier to maintain, restyle, and scale.

In [*Chapter 2*](#02.xhtml#_idTextAnchor098), *HTML – Structuring Your Web Pages*, you created pages using headings, paragraphs, links, images, lists, tables, and forms. At that point, the browser decided how everything looked. Fonts followed browser defaults, spacing was basic, and everything was stacked vertically. CSS is how you take control away from the browser and make deliberate design choices.

CSS works by defining rules. Each rule says "select these elements" and "apply these styles to them". Once you understand that sentence, you understand the core of CSS.

## Adding CSS to a web page

There are three ways to apply CSS to HTML. You will see all three in real projects, but only one of them is recommended for most sites. They are:

* **Inline CSS**: Style defined at the element level.
* **Internal CSS**: Style defined at the page level.
* **External CSS**: Style defined at the website level.

Let's see examples of each.

**Inline CSS** is written directly on an HTML element:

```
<p style="color: red;">This text is red.</p>
```

This works, but it is messy. Styles get scattered throughout your HTML, they are hard to reuse, and they are painful to maintain.

**Good practice**: Avoid inline CSS except when experimenting.

**Internal CSS** is written inside a `<style>` tag in the `<head>` section of the page:
