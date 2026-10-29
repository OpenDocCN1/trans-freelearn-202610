# Listing files and directories in a directory

Java's method `listFile` in the `java.io.File` class lists files and directories directly under the specified directory:

```
(doseq [x (.listFiles (clojure.java.io/file "."))]
  (if (.isDirectory x) (print "[D] ") (print "[F] "))
  (println (.getPath x))
)
[F] ./CHANGELOG.md
[D] ./images
[F] ./.nrepl-port
[F] ./README.md
[D] ./test
[F] ./project.clj
[F] ./chapter04.md
[D] ./src
[D] ./doc
[D] ./dev-resources
[F] ./.gitignore
[D] ./target
[F] ./.hgignore
[D] ./resources
[F] ./LICENSE
nil
```

Clojure's function `file-seq` lists files recursively:

```
(doseq [x (file-seq (clojure.java.io/file "."))]
  (if (.isDirectory x) (print "[D] ") (print "[F] "))
  (println (.getPath x))
)
[D] .
[F] ./CHANGELOG.md
[D] ./images
[F] ./images/rabbitmq.png
[F] ./images/kafka-topic.png
[F] ./images/kafka.png
[F] ./images/zookeeper.png
[F] ./.nrepl-port
[F] ./README.md
[D] ./test
[D] ./test/chapter04
[F] ./test/chapter04/core_test.clj
[F] ./project.clj
```