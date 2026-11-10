# 24

# Configurations and
Customizations Using WDK

Till now we have seen how to create Docbase objects and manage them via the user-friendly interface provided by default Web Publisher screens. We have seen how content files are created through the New Content screen in Web Publisher and how properties of existing objects are modified through the Properties (attributes) screen. In fact all other screens that we have seen in Web Publisher such as the Categories page displaying existing Web Publisher categories, the Inbox screen displaying workflow tasks, and other server notifications, etc. are all default screens provided by the Web Publisher application. Webtop, Digital Asset Manager, and Documentum Administrator are examples of some other web-based applications in Documentum that have their own specific user interfaces.

What if you need to modify the look-and-feel of these default screens or alter some basic functionality associated with these screens? Welcome to the world of Documentum WDK (short for Web Development Kit)!

WDK (as we discussed in Chapter 3) is a framework that Documentum has provided to build and deploy J2EE web applications for connecting with Documentum Content Server. WDK consists of a set of reusable and extensible server-side Java components that interact with Content Server via DFC calls.

Webtop is a reference implementation provided by Documentum using the WDK framework for basic content management functionalities. Web Publisher and the Documentum Administrator tools that we have covered in this book have been built over Webtop. Using WDK, you can alter the user interface of Web Publisher/Documentum Administrator screens and even extend and modify their functionalities via customizations. The WDK framework has been written well and provides flexibility to override default functionality and add your own custom components as well!

Using WDK, you can primarily perform two kinds of tasks:

+   - **Configuration:** This includes simple operations such as introducing new menu options, modifying labels and texts in screens, altering look-and-feel such as images associated with button controls, and hiding/displaying attributes/fields in various screens. Using XML configuration files and JSPs (Java Server Pages) provided in the WDK, you can achieve simple configurations of web applications.