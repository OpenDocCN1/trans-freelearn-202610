method loads a Clojure script, and executes a given method; here, we will call the namespace ex1, and the method main:

```
(ns vaadin2.vaadin2ui
  (:gen-class
    :name vaadin2.vaadin2ui
    :extends com.vaadin.ui.UI))
(defn -init
  [main-ui request]
  (clojure.lang.RT/loadResourceScript "ex1.clj")
  (.setContent
    main-ui
    (.invoke (clojure.lang.RT/var "ex1" "main") main-ui)))
```

Finally, you need to write the ex1/main function, where the main function sets the content of the main placeholder.

Here, you can make use of the Vaadin classes at will; this is where it starts. An application usually needs a layout, and our first label will say something like hello in French:

```
(ns ex1
  (:import [com.vaadin.ui VerticalLayout Label]))
(defn main[ui]
  (let [
    layout  (VerticalLayout.)
    label   (Label. "Bonjour le monde!")]
    (.addComponent layout label)))
```

All the elements are in place; the Leiningen servlet plugin will give you a servlet subcommand, including a run command to start the server:

```
lein servlet run
```

In the default development mode, that command will also open a browser page with the right URL, and here you go, some hello world in French!