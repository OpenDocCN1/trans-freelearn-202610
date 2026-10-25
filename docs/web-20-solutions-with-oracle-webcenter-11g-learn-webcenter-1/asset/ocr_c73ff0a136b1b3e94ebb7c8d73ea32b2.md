# # 4 Getting Started with WebCenter Development: Hello World(s)

Before we jump into developing our Enterprise 2.0 Intranet, we will create a simple WebCenter application to demonstrate the tools, development methodology, and deployment of custom WebCenter applications.

This chapter will go through detailed step-by-step instructions to provide you with the knowledge you will need to succeed while implementing the example throughout the rest of the book, and on your own project.

I'm sure you noticed that the title of this chapter contains the phrase "Hello World(s)", where the "(s)" sticks out like a sore thumb. This is intentional, but not to say we are building multiple WebCenter examples, nor is it to subliminally proclaim and acknowledge the existence of an alien race. The fact is that a single WebCenter Hello World application is best exemplified in its entirety when built up by a few Hello World components. We will show you a web service, consumed by a portlet, consumed by a WebCenter application.

+   - **HelloWorldWS**: A simple web service that exposes a single method to say hello to an input name
+   - **HelloWorldPortlet**: A Standards-based Java Portlet that uses the HelloWorldWS web service