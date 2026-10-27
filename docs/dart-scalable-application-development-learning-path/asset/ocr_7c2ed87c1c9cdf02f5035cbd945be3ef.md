A simple titled slide with bullet points would be defined as:

```
```text
#Dart Language
+Created By Google
+Modern language with a familiar syntax
+Structured Web Applications
+It is Awesomely productive!
```
```

I am positive you only had to read that once! This will translate into the following HTML:

```
```html
<h1>Dart Language</h1>
<li>Created By Google</li>s
<li>Modern language with a familiar syntax</li>
<li>Structured Web Applications</li>
<li>It is Awesomely productive!</li>
```
```

Markdown is very easy and fast to parse, which probably explains its growing popularity on the Web. It can be transformed into many other formats.

# Parsing the presentation

The content of the TextAreaHtml element is split into a list of individual lines, and processed in a similar manner to some of the features in the text editor application using forEach to iterate over the list. Any lines that are blank once any whitespace has been removed via the trim method are ignored:

```
```text
#A New Slide Title
+The first bullet point
+The second bullet point

#The Second Slide Title
+More bullet points
!http://localhost/img/logo.png
#Final Slide
+Any questions?
```
```

For each line starting with a # symbol, a new slide object is created.

For each line starting with a + symbol, they are added to this slide's bullet point list.

For each line discovered using a ! symbol, the slide's image is set (a limit of one per slide).

This continues until the end of the presentation source is reached.