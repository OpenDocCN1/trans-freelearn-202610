```
(with-open [o (writer "data-set-1.xml")]
  (doseq [i (range 1 10)]
    (.write o (str (element :friend {"age" "23"} (str "Friend" i))))
    (.write o "\n")))
```

Note that this can also be done in parallel with a basic pmap call. While the data order is lost in a way, the data is generated way more rapidly:

```
(with-open [o (writer "test_1.xml")]
  (doall
    (pmap
      #(.write o
        (str
          (element :friend
            {"age" "23"}
            (str "Friend" %)) "\n"))
      (range 1 10))))
```

Note that in this case, we need to use doall to force the execution before getting out of the scope of with-open, otherwise we will get a stream closed exception.

While this is generating a large file with one XML per line, a proper XML file would look like the following:

```
(with-open [o (writer "test_2.xml")]
(.write o "<friends>\n")
(doseq [i (range 1 10)]
  (.write o
    (str
      (element :friend
        {"age" "23"}
        (str "Friend" i))
      "\n"))))
(.write o "</friends>\n"))
```

Now increase the range value to about 10,000,000 to get a nice 500 MB file of data. You can also note that the memory usage did not increase so much and stayed relatively stable.

Let's now analyze the data back in!