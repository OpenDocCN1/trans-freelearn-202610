```
htmlText = intrawikiR.sub('<a href="\\g<link>">\\g<link></a>', htmlText)
    htmlText = externalLinkR.sub('<a href="\\g<link>">\\g<description></a>', htmlText)
    return htmlText

def html(self):
    results = self.header()
    results += """
    <!-- BEGIN main content -->
    """
    results += self.wiki_to_html()
    results += """
    <!-- END main content -->
    """
    results += self.footer()
    return results
```

Page holds the title of the article, the wiki text associated with it, and a handle on the controller. Most of the code in this class is used to support html(), a function used to render this page in an HTML format.

![](bbox=[0.21, 0.5, 0.26, 0.55])

> Most web frameworks encourage usage of templates to avoid embedding HTML in the application. This helps decouple the information shown on the pages from the format it is displayed in. Since this chapter's focus is on security and not clean Model-View-Controller (MVC) tactics, the HTML is embedded directly into the application.

Our wiki page basically generates three parts: a header, some wiki text converted into HTML, followed by a footer(). The wiki text rules are simple for our example:

+   - It supports the free linking style of Wikipedia, where `[[article to link to]]` maps to local web path `/article_to_link_to`. You can also insert a pipe followed by an alternate block of text to be displayed `[[article to link|my alt text]]`.
+   - External links `[http://springpythonbook.com Spring Python book site]` map to the full URL, with the text after the first space being displayed on the page

While it would be easy to implement more wiki format rules, we want to stick with demonstrating security for our web application.