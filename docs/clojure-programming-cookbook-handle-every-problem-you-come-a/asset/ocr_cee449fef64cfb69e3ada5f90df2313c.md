```
(let [filename "test.txt" file  (System.IO.StreamWriter.
filename)]
    (.WriteLine file "===Hello Clojure ===")
    (.Close file))
```

Avoiding all explicit call to .NET, you could also retrieve the content of the newly created files using the slurp function:

```
user=> (println (slurp "test.txt"))
; ===Hello Clojure ===
```

# Parsing some XML using C# code

We will quickly look at an example on how to parse XML from Clojure on .NET because this will be the base of our integration example in the next section. For this example, we will load the System.Xml library, as we have seen before, using the following:

```
(System.Reflection.Assembly/LoadWithPartialName "System.Xml")
; #object[RuntimeAssembly 0xf097c6 "System.Xml,
Version=4.0.0.0, Culture=neutral,
PublicKeyToken=b77a5c561934e089"]
```

A note for later would be to confirm that the DLL is loaded properly, as seen in the echo message when typed on the console.
LoadWithPartialName outputs some information showing that, from that newly loaded library, we will import a few classes, the same ones we used with Java interop. The ns declaration makes it easy to hold all of those together so:

```
(ns hello.parsing
  (:import [System.Xml XmlReader])
  (:import [System.IO StringReader])
  (:import [System.Text]))
```

Here, you can note how the XML definition is completely defined through the usual Clojure code:

```
(def xml "<bookstore>
        <book genre='autobiography' publicationdate='1981-03-22' ISBN='1-861003-11-0'>
            <title>The Autobiography of Benjamin Franklin</title>
```