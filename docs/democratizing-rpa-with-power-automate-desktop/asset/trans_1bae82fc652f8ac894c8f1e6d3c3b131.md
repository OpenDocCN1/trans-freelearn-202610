

to probably the most common and well-known protocols FTP and HTTP, before we take a closer look at the different data formats for the web

## FTP

Not so common nowadays, FTP was and still is a very efficient way to upload and download folders and files to and from a server. PAD is also able to use this protocol and access an FTP server. The following actions are provided within the FTP actions group:

- **Open / Open secure / Close connection**: These are required to establish a connection to the FTP server.
- **List directory / Change working directory**: Comparable to **DIR / CD command** in Windows command prompt to list the content of the current directory or change the current directory.
- **Create / Delete / Synchronize directory**: These actions do exactly what they are named after. We will look at the synchronization action at the end of this list.
- **Upload / Download / Delete / Rename FTP file(s)**: These are self-explanatory and simple to use.
- **Invoke FTP command**: Several FTP commands can be issued on the FTP server, such as FEAT (list extended features) or STAT (returns information about a current file transfer).

The certainly most interesting action here is the **Synchronize directory** action. Instead of comparing remote files and directories with different other actions, this one is really a big time-saver, but it can only work in one direction at a time. It offers the following action parameters:

## Synchronize directories

Synchronize the files and subdirectories of a given folder with a given remote FTP directory More info

## Select parameters

FTP connection: %FTPConnection%

Synchronization direction: Remote -> Local (Download)

Files to sync: All files

Local folder:

FTP directory: /

Delete if source is absent:

Include subdirectories:

Time difference in hours: 0

Time difference in minutes: 0

Time difference ahead:

Variables produced FilesAdded FilesModified FilesDeleted

On error

Save Cancel

# Figure 11.6 – Synchronize FTP directories with PAD

As you can see in the previous screenshot, this action provides all the setting options that are required for directory synchronization. The option to
specify one or more filters for the files brings a lot of flexibility. The action produces three result lists, which can be used in the following:

- `FilesAdded`
- `FilesModified`
- `FilesDeleted`

While FTP was primarily designed for file exchange, the following protocol is responsible for the vast majority of internet traffic: HTTP.

## HTTP

HTTP is the standard protocol on which most traffic on the internet is based. The PAD actions in this group represent API calls that PAD can perform, as well as file upload and download. Any API call works according to the request-response principle, where different methods (verbs) are available to describe the desired action. The most common methods used in such requests are the following:

- `GET`: This allows querying content via the URL
- `POST`: This is used for requests where the supplied contents are to be processed in a target resource, for example, saving a data record or other transactions
- `PATCH`: This represents a request for the target address to update the partial aspects transmitted

API services are very commonly used when companies want to provide their services and data controllable electronically or interact with other services. Modern web APIs thus represent the interface to the data and processes with which electronic communication can take place. While many such services use standard methods (as described previously) and are based on the **representational state transfer (REST)** principle, some services use the **Simple Object Access Protocol (SOAP)** protocol. SOAP
was created to establish a standard for communication between the client and server. However, SOAP is (contrary to its name) more elaborate and complex to implement and expects a predefined structure. This and other factors have led to a tendency for new web services to be built based on the REST architecture. In PAD, of course, we have corresponding actions for both versions that can be used for this, plus another action:

- **Download from web:** Used if we want to download a specific text or a file from a web server.
- **Invoke SOAP web service:** This is the action to use if we need to communicate with a web service that is based on SOAP. This action also provides a little wizard that helps to determine the values for the different parameters.
- **Invoke web service:** This action calls a RESTful web service.

PAD manages to hide the possible complexity in web service communication. Sometimes, however, more advanced settings have to be made because, for example, certain settings, such as authentication, are expected. Therefore, the two actions for web service calls can also make advanced settings, which are stored in the **Advanced** area, such as **Connection timeout, User agent, Encoding**, and more.

## Working with web data formats

When applications communicate with each other via web services, this is also done using a language. REST-based web services use JSON as a language here, which is built-in to PAD. There is, for example, an action to convert a variable into JSON and back.

## Background information – custom object versus JSON object

Internally, PAD can handle JSON objects but prefers to use a so-called custom object. This is a more flattened structure that is easier to handle in loops and contains a list of attribute names and values. A JSON object, instead, can be of any complexity with multiple levels of nesting structures.

It is possible to convert such a structure into a custom object. In this case, the custom object contains this flattened structure with nested objects as values.

Another very well-known concept is markup languages. The two best-known representatives here are **HyperText Markup Language (HTML)** and **XML**. HTML is used to represent content and web pages, and XML is for the structured storage of data or configuration information but also for communicating with web services. SOAP, as mentioned previously, communicates via XML. The basic structure of a simple XML file is depicted in the following diagram:

![](img/ce6bfe9c712dcd60fc2c4e058dc84e6a_326_0.png)

Figure 11.7 – XML structure explained

There is also a special notation to describe the way to an element or a value within an XML document. This is called XPath and looks like this: `/myfirstelement/subelement1`. This would give us the value of `subelement1`, which is `value of the element`. This is also referred to as an XPath query. PAD also includes actions to handle XML:

- **Read / Write XML file**: This handles the XML file that should be used in the UI flow.
- **Insert / Remove XML element**: This allows us to add a new element at a specific position of the XML document, which is provided by an XPath expression. The XML element to insert can also be a larger structure as long as there is only one root element. To remove an element, an XPath expression has to be provided.

- **Get / set XML element value**: As can be seen in the previous example, an XPath expression can also be used here to read or set the value of an element.
- **Get / set / remove XML element attribute**: These correspond to the operations that can also be performed for the XML elements, only here with the attributes.
- **Execute XPath expression**: This provides an XPath expression to select or even search for specific elements (nodes) or attributes. Please refer to the *Further reading* section to find out more on this topic.

To further complete the list of data formats, of course, a specific document format must be defined. Adobe PDF is the de facto standard for generating and exchanging documents. These are characterized by many important features that are required in the internet-based communication world:

- They can be read from almost any device and browser
- They are considered safe (do not contain viruses or malicious code)
- They cannot be modified or changed if the author does not allow it
- Their creation and consumption are free of charge

In particular, because in the context of business applications, we usually also work with documents and PDFs, PAD has a set of actions that allow working with these types of documents. These include the following:

- **Extract text / tables / images from PDF**: These actions extract content from a PDF file. The images for example can be extracted and stored in a specific location of the workstation.
- **Extract PDF file pages to new PDF file**: This action allows us to create a new PDF document from the specific pages of another document.
- **Merge PDF files**: This takes a list of PDF documents and creates one combined document.

