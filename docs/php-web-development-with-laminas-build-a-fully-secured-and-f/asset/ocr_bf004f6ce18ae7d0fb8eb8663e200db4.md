adapters, which enable your application to save and read data from filesystem, Memcached, Redis, or MongoDB databases, among others.

# CLI

Although we have used Laminas to develop web applications, the framework is not limited to this purpose. In fact, the PHP manual says that PHP can do anything. With Laminas, we can do anything, but with more control.

> **What can PHP do?**
> You can find a description of the possible uses of PHP at https://www.php.net/manual/en/intro-whatcando.php.

Believe it or not, you can create command-line PHP programs with Laminas. To do this, you should use the laminas-cli component.

# Crypt

In our whatstore project, we said that it is important to encrypt passwords. But you shouldn’t use the algorithms that were used in the examples in this book. It is recommended to use strong cryptography and password hashing to protect and authenticate sensitive data. The laminas-crypt component offers an easy and secure way to implement cryptography in PHP applications. This component was developed by several great applied cryptography specialists, such as Enrico Zimuel, a core member of the PHP Framework Interoperability Group.

# CSRF

There is actually no component called laminas-crsf. However, there are two classes that handle CSRF. CRSF stands for cross-site request forgery, an attack where someone tries to send malicious data through falsified forms. The laminas-form component provides Element\\Csrf to add a hash element to a form with timeout configuration. To validate the hash element value, you can use the Laminas\\Validator\\Csrf class.

# DOM

DOM stands for document object model. This is a web standard, documented at https://www.w3.org/DOM/DOMTR. According to this standard, the HTML tags can be handled as an object hierarchy. In addition, it is possible to search for and select objects using the XPath language and a CSS selector. The laminas-dom component enables you to find elements in XML and HTML documents using these two approaches. It is very useful to automate the reading of documents in these formats in order to import data or search for data on websites through the implementation of web crawlers.