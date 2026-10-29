```
<author>
    <first-name>Benjamin</first-name>
    <last-name>Franklin</last-name>
</author>
<price>8.99</price>
</book>
</bookstore>")
```

Finally, we will load the XmlReader from a C# StringReader, and then using some obscure C# Sax parsing methods we can access the name of the book:

```
(let [reader (XmlReader/Create (StringReader. xml))]
  (.ReadToFollowing reader "title")
  (println
    (.ReadElementContentAsString reader)))
; The Autobiography of Benjamin Franklin
```

That was rather concise! In the next example, we will take it a step further by calling a remote web service.

# Calling a REST API using Clojure/C#

In this example, we will take it from the XML parsing from the previous example to parse a response from a remote web service. The remote web service will be Yahoo Weather, and we will make it so that we can find the sunset time for a specific location.

If you are starting from a fresh REPL, or at the top of your file, start by importing the XML Library as done previously:

```
(System.Reflection.Assembly/LoadWithPartialName "System.Xml")
; #object[RuntimeAssembly 0xf097c6 "System.Xml,
Version=4.0.0.0, Culture=neutral,
PublicKeyToken=b77a5c561934e089"]
```

On top of the XML parsing-related classes, we will import the C# WebClient, which will send the HTTP request for us:

```
(ns hello.net
  (:import [System.Net WebClient])
  (:import [System.Xml XmlReader])
  (:import [System.IO StringReader]))
```