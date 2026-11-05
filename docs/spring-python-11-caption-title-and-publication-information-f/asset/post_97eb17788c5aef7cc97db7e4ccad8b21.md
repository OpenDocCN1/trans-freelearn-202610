
![](img/bcf886d8b665372ee8afbb620d4273bb_16_0.png)

Warnings or important notes appear in a box like this.

![](img/bcf886d8b665372ee8afbb620d4273bb_16_1.png)

Tips and tricks appear like this.

## Reader feedback

Feedback from our readers is always welcome. Let us know what you think about this book – what you liked or may have disliked. Reader feedback is important for us to develop titles that you really get the most out of.

To send us general feedback, simply send an email to feedback@packtpub.com, and mention the book title via the subject of your message.

If there is a book that you need and would like to see us publish, please send us a note in the SUGGEST A TITLE form on www.packtpub.com or email suggest@packtpub.com.

If there is a topic that you have expertise in and you are interested in either writing or contributing to a book on, see our author guide on www.packtpub.com/authors.

## Customer support

Now that you are the proud owner of a Packt book, we have a number of things to help you to get the most from your purchase.

![](img/bcf886d8b665372ee8afbb620d4273bb_16_2.png)

**Downloading the example code for the book**
Visit https://www.packtpub.com//sites/default/files/downloads/0660_Code.zip to directly download the example code.
The downloadable files contain instructions on how to use them.

## Errata

Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you find a mistake in one of our books—maybe a mistake in the text or the code—we would be grateful if you would report this to us. By doing so, you can save other readers from frustration, and help us to improve subsequent versions of this book. If you find any errata, please report them by visiting http://www.packtpub.com/support, selecting your book, clicking on the **let us know** link, and entering the details of your errata. Once your errata are verified, your submission will be accepted and the errata added to any list of existing errata. Any existing errata can be viewed by selecting your title from http://www.packtpub.com/support.

## Piracy

Piracy of copyright material on the Internet is an ongoing problem across all media. At Packt, we take the protection of our copyright and licenses very seriously. If you come across any illegal copies of our works, in any form, on the Internet, please provide us with the location address or web site name immediately so that we can pursue a remedy.

Please contact us at copyright@packtpub.com with a link to the suspected pirated material.

We appreciate your help in protecting our authors, and our ability to bring you valuable content.

## Questions

You can contact us at questions@packtpub.com if you are having a problem with any aspect of the book, and we will do our best to address it.

## 1 Getting Started with Spring Python

Spring Python takes the concepts of the Spring Framework and Spring Security, and brings them to the world of Python. It isn't a simple line-by-line port of the code. Instead, it takes some powerful ideas that were discovered in the realm of Java, and pragmatically applies them in the world of Python.

Spring (Java) provides many simple, easy-to-use functional parts to assemble applications instead of a monolithic framework to extend. Spring Python uses this same approach. This means we can use as little or as much Spring Python as we need to get the job done for each Python application.

In this chapter, we will learn:

- About Spring Python's non-invasive API which makes it easy to use other libraries without having to make major changes to your own code base
- How Spring Python uses inversion of control to decouple object creation from object usage to empower the developer
- How Spring Python provides the means to help professional Python developers by offering a non-invasive API to easily access advanced services
- The ways in which Spring Python offers professional Java developers an easy way to mix Python and Java together through the combination of Python/Jython/Java
- How to install the library from both binary and source code
- How extensible Spring Python is, and also some links to the Spring Python community

### Spring Python for Python developers

You have already picked one of the most popular and technically powerful dynamic languages to develop software, Python. Spring Python makes it even easier to solve common problems encountered by Python developers every day.

### Exploring Spring Python's non-invasive nature

Spring Python has a non-invasive nature, which means it is easy to adopt the parts that meet your needs, without rewriting huge blocks of code. For example, Pyro (http://pyro.sourceforge.net) is a 3rd party library that provides an easy way to make remote procedure calls.

In order to demonstrate the Spring way of non-invasiveness, let's code a simple service, publish it as a web service using Pyro's API, and then publish it using Spring Python's wrapper around Pyro. This will show the difference in how Spring Python simplifies API access for us, and how it makes the 3rd party library easier to use without as much rework to our own code.

- 1. First, let's write a simple service that parses out the parameters from a web request string:
```python
class ParamParser(object):
    def parse_web_parms(self, parm):
        return [tuple(p.split("=")) for p in parm.split("&")]
```
- 2. Now we can write a simple, functional piece of code that uses our service in order to have a working version.
```python
parser = ParamParser()
parser.parse_web_parms("pages=5&article=Spring_Python")
```
This is just instantiating the `ParamParser` and accessing the function. To make this a useful internet service, it needs to be instantiated on a central server and should be configured to listen for calls from clients.
- 3. The next step is to advertise it as a web service using the API of Pyro. This will make it reachable by multiple Pyro clients. To do this, we define a daemon which will host our service on port 9000 and initialize it.
```python
import Pyro

daemon = Pyro.core.Daemon(host="localhost", port="9000")
Pyro.core.initServer()
```
- 4. Next, we create a `Pyro` object instance to act as proxy to our service as well as an instance of our `ParamParser`. We configure the proxy to delegate all method calls to our service.
```
pyro_proxy = Pyro.core.ObjBase()
parser = ParamParser()
pyro_proxy.delegateTo(parser)
```
- 5. Finally, we register the `pyro_proxy` object with the `daemon`, and startup a listen-dispatch loop so that it's ready to handle requests:
```
daemon.connect(pyro_proxy, "mywebservice")
daemon.requestLoop(True)
```
When we run this server code, an instance of our `ParamParser` will be created and advertised at `PYROLOC://localhost:9000/mywebservice`.

To make this service complete, we need to create a `Pyro` client that will call into our service. The proxy seamlessly transfers Python objects over the wire using the `Pyro` library, in this case the tuple of request parameters.
```
import Pyro

url_base = "PYROLOC://localhost:9000"
client_proxy = Pyro.core.getProxyForURI( \
    url_base + "/mywebservice")
print client_proxy.parse_web_parms( \
    "pages=5&article=Spring_Python")
```
The `Pyro` library is easy to use. One key factor is how our `ParamParser` never gets tightly coupled to the `Pyro` machinery used to serve it to remote clients. However, it's very invasive.

What if we had already developed a simple application on a single machine with lots of methods making use of our utility? In order to convert our application into a client-server application, we would have to rewrite it to use the `Pyro` client proxy pattern everywhere that it was called. If we miss any instances, we will have bugs that need to be cleaned up. If we had written automated tests, they would also have to be rewritten as well. Converting a simple, one-machine application into a multi-node application can quickly generate a lot of work.

That is where Spring Python comes in. It provides a different way of creating objects which makes it easy for us to replace a local object with a remoting mechanism such as `Pyro`. Later on, we will explore the concepts of Spring Python's container in more detail.
