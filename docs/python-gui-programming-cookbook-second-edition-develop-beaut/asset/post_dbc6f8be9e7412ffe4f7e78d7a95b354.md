
In the write_to_scrol function, we will use the same loop as before, but now we will send the messages to the TCP server. The server modifies the received message and then sends it back to us. Next we place it into the GUI member queue, which, as in the previous recipes, runs in its own thread:

```python
sock.send(b'Message from a queue: ' + bytes(str(idx).encode()) )
```

Note the b before the string and then, well, all the rest of the required casting.

We start the TCP server in its own thread in the initializer of the OOP class:

```python
class OOP():
    def __init__(self):
        # Start TCP/IP server in its own thread
        svrT = Thread(target=startServer, daemon=True)
        svrT.start()
```

In Python 3 we have to send strings over sockets in binary format. Adding the integer index now becomes a little bit convoluted as we have to cast it to a string, encode it, and then cast the encoded string into bytes!

Clicking the Click Me! button on Tab 1 now creates the following output in our ScrolledText widget as well as on the console, and the response due to the use of threads is very fast:

GUI_TCP_IP.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_222_0.png)

#### How it works...

We created a TCP server to simulate connecting to a server in our local area network or on the Internet. We turned our queues module into a TCP client. We are running both the queue and the server in their own background thread, which keeps our GUI very responsive.

### Using urlopen to read data from websites

This recipe shows how we can easily read entire web pages by using Python's built-in modules. We will display the web page data first in its raw format and then decode it, and then we will display it in our GUI.

#### Getting ready

We will read the data from a webpage and then display it in the `ScrolledText` widget of our GUI.

#### How to do it...

First, we create a new Python module and name it `URL.py`. We then import the required functionality to read webpages using Python. We can do this in very few lines of code.

We wrap our code in a `try...except` block similar to Java and C#. This is a modern approach to coding, which Python supports. Whenever we have code that might not complete, we can experiment with this code and, if it works, all is fine. If the block of code in the `try...except` block does not work, the Python interpreter will throw one of several possible exceptions, which we can then catch. Once we have caught the exception, we can decide what to do next.

There is a hierarchy of exceptions in Python and we can also create our own classes that inherit from and extend the Python exception classes. In the following code, we are mainly concerned that the URL we are trying to open might not be available so, we wrap our code within a `try...except` code block. If the code succeeds in opening the requested URL, all is fine. If it fails, maybe because our Internet connection is down, we fall into the exception part of the code and print out that an exception has occurred:

```python
from urllib.request import urlopen
link = 'http://python.org/'

try:
    http_rsp = urlopen(link)
    print(http_rsp)
    html = http_rsp.read()
    print(html)
    html_decoded = html.decode()
    print(html_decoded)
except Exception as ex:
    print('*** Failed to get Html! ***\n\n' + str(ex))
else:
    return html_decoded
```

You can read more about Python exception handling at https://docs.python.org/3.6/library/exceptions.html.

By calling urlopen on the official Python website, we get the entire data as one long string. The first print statement prints this long string out to the console.

We then call decode on the result, and this time we get a little over 1,000 lines of web data, including some whitespace.

We also print out the type for calling urlopen, which is an http.client.HTTPResponse object. Actually, we print it out first:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_224_0.png)

Here is the official Python webpage we just read. If you are a web developer, you probably have some good ideas about what to do with the parsed data:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_225_0.png)

We then display this data in our GUI inside the `ScrolledText` widget. In order to do so, we have to connect our new module, which reads the data from the webpage to our GUI.

In order to do this, we need a reference to our GUI, and one way to do this is by tying our new module to the **Tab 1** button callback. We can return the decoded HTML data from the Python webpage to the `Button` widget, which we can then place into the `ScrolledText` control.

So, let's turn our code into a function and return the data to the calling code:

```python
from urllib.request import urlopen
link = 'http://python.org/'

def get_html():
    try:
        http_rsp = urlopen(link)
        print(http_rsp)
        html = http_rsp.read()
        print(html)
        html_decoded = html.decode()
        print(html_decoded)
    except Exception as ex:
        print('*** Failed to get Html! ***\n\n' + str(ex))
    else:
        return html_decoded
```

We can now write the data from our button callback method to the ScrolledText control by first importing the new module and then inserting the data into the widget. We also give it some sleep after the call to write_to_scrol:

```python
import Ch06_Code.URL as url

# Button callback
def click_me(self):
    self.action.configure(text='Hello ' + self.name.get())
    bq.write_to_scrol(self)
    sleep(2)
    html_data = url.get_html()
    print(html_data)
    self.scrol.insert(tk.INSERT, html_data)
```

The HTML data is now displayed in our GUI widget:

GUI_URL.py

![](img/986b0dbf3f539f4f3baa82bd3003f04e_227_0.png)

#### How it works...

We create a new module to separate the code that gets the data from a webpage from our GUI code. This is always a good thing to do. We read in the webpage data and then return it to the calling code after decoding it. We then use the button callback function to place the returned data into the `ScrolledText` control.

This chapter introduced us to some advanced Python programming concepts, which we combined to produce a functional GUI program.

## 7 Storing Data in our MySQL Database via our GUI

In this chapter, we will enhance our Python GUI by connecting to a MySQL database. We will cover the following recipes:

- Installing and connecting to a MySQL server from Python
- Configuring the MySQL database connection
- Designing the Python GUI database
- Using the SQL INSERT command
- Using the SQL UPDATE command
- Using the SQL DELETE command
- Storing and retrieving data from our MySQL database
- Using the MySQL workbench

### Introduction

Before we can connect to a MySQL server, we must have access to a MySQL server.

![](img/986b0dbf3f539f4f3baa82bd3003f04e_229_0.png)

The first recipe in this chapter will show you how to install the free MySQL Server Community Edition.

After successfully connecting to a running instance of our MySQL server, we will design and create a database that will accept a book title, which could be our own journal or a quote we found somewhere on the Internet. We will require a page number for the book, which could be blank, and then, we will `insert` the quote we like from a book, journal, website, or a friend into our MySQL database using our GUI, built using Python 3.6 and above.

We will insert, modify, delete, and display our favorite quotes using our Python GUI to issue these SQL commands and to display the data.

![](img/986b0dbf3f539f4f3baa82bd3003f04e_230_0.png)

**CRUD** is a database term you might come across, which abbreviates the four basic SQL commands and stands for **Create, Read, Update, and Delete.**

Here is the overview of Python modules for this chapter:

![](img/986b0dbf3f539f4f3baa82bd3003f04e_230_1.png)

### Installing and connecting to a MySQL server from Python

Before we can connect to a MySQL database, we have to connect to the MySQL Server. In order to do this, we need to know the IP address of the MySQL server as well as the port it is listening on.

We also have to be a registered user with a password in order to get authenticated by the MySQL server.

#### Getting ready

You will need to have access to a running MySQL Server instance and you also need to have administrator privileges in order to create databases and tables.

There is a free MySQL Community Edition available from the official MySQL website. You can download and install it on your local PC from http://dev.mysql.com/downloads/windows/installer/5.7.html:
