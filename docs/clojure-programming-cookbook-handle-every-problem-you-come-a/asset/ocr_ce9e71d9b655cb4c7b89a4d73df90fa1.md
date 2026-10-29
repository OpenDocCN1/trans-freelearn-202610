```
$ mvn clojure:repl
[WARNING]
[WARNING] Some problems were encountered while building the effective settings
[WARNING] Expected root element 'settings' but found 'pluginGroups' (position: START_TAG seen <pluginGroups>... @1:14
) @ /home/makoto/.m2/settings.xml, line 1, column 14
[WARNING]
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Building chapter05 0.1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] >>> clojure-maven-plugin:1.3.10:repl (default-cli) > test-compile @ chapter05 >>>
[INFO] --- build-helper-maven-plugin:1.7:add-source (add-source) @ chapter05 ---
[INFO] Source directory: /home/makoto/clojure/clojure-packt-book/chapter05/src/java added.
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ chapter05 ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ chapter05 ---
[INFO] Changes detected - recompiling the module!
[WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
[INFO] Compiling 8 source files to /home/makoto/clojure/clojure-packt-book/chapter05/target/classes
[INFO] --- clojure-maven-plugin:1.3.10:compile (compile) @ chapter05 ---
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ chapter05 ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 0 resource
[INFO] --- maven-compiler-plugin:3.1:testCompile (default-testCompile) @ chapter05 ---
[INFO] Nothing to compile - all classes are up to date
[INFO] <<< clojure-maven-plugin:1.3.10:repl (default-cli) < test-compile @ chapter05 <<<
[INFO] --- clojure-maven-plugin:1.3.10:repl (default-cli) @ chapter05 ---
Clojure 1.8.0
user=>
```