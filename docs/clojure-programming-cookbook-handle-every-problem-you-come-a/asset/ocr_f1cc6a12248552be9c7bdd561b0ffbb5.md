```
:exclusions [cljsjs/react]]
[org.omcljs/om "1.0.0-alpha37"]
```

# Tips for dependencies

In this recipe, we will use version 1.0.0-alpha37 of om, which supports om.next. However, using the figwheel template generates version 0.3.6 of the sablono dependency and it uses the older version of react, which causes a runtime problem of om.next.

To solve this problem, we have checked the dependencies of libraries. The \ command finds confusing dependencies as follows:

```
Possibly confusing dependencies found:
[sablono "0.3.6"] -> [cljsjs/react "0.13.3-0"]
overrides
[org.omcljs/om "1.0.0-alpha37"] -> [cljsjs/react-dom "15.0.1-1"] -> [cljsjs/react "15.0.1-1"]
and
[org.omcljs/om "1.0.0-alpha37"] -> [cljsjs/react "15.0.1-1"]
.....
[org.omcljs/om "1.0.0-alpha37"]
[cljsjs/react-dom "15.0.1-1"]
.....
[sablono "0.3.6"]
[cljsjs/react "0.13.3-0"]
.....
```

That is the reason why we have excluded cljsjs/react from the sablono dependency as follows:

```
[sablono "0.3.6"
    :exclusions [cljsjs/react]]
```

# Starting figwheel

Start the following command and wait for a prompt for ClojureScript code:

```
$ lein figwheel
```

# How to do it...