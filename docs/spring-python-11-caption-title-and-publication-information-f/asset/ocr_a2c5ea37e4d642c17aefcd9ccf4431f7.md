# Building web applications ignoring security

We need a simple application. For this example, we will use CherryPy (http://cherrypy.org), a pythonic web framework that conveniently maps URLs into Python methods.

> If you want to know more about the CherryPy framework, I highly recommend reading *CherryPy Essentials* by Sylvain Hellegouarch.

After building our web application, we will review some of the options people take in securing things. Then, we will plug in Spring Python Security, showing how easy it is to lock down an application.

First, let's build a simple web application that serves wiki pages, and allows us to edit them.

```
import cherrypy

def forward(url):
    return '<META HTTP-EQUIV="Refresh" CONTENT="0; URL=' + url + '">'

class Springwiki(object):
    def __init__(self, controller = None):
        self.controller = controller

    @cherrypy.expose
    def index(self, article="Main Page"):
        page = self.controller.getPage(article)
        return page.html()

    @cherrypy.expose
    def default(self, article="Main Page"):
        return self.index(article)

    @cherrypy.expose
    def submit(self, article, wpTextbox=None, wpSummary=None):
        self.controller.updatePage(article, wpTextbox, wpSummary)
        return forward("/" + article)

    @cherrypy.expose
    def edit(self, article):
        page = self.controller.getEditPage(article)
        return page.html()
```