```
(defn -main [& args]
  (println (get-sunset (first args) "jp")))
```

We also make sure that the :gen-class directive is present in the namespace definition:

```
(ns hello.net
  (:gen-class))
```

From the command line, the expected outcome is that, taking a city as a parameter, the runtime will call the Yahoo API. Let's check that with the run command of the Leiningen CLR plugin:

```
lein clr run -m hello.net "Osaka"
; 5:34 pm
```

You are good. (You are right?) Now to make this reusable, we will make use of the compile command of the Leiningen CLR plugin to compile one namespace:

```
lein clr compile hello.net
```

Note that this will compile the required namespaces as well. If all goes well, in the target\clr\bin folder, you now have a few more files, notably the two newly generated ones - the DLL and the EXE files:

```
-a----      2016/02/09      17:21          10240
hello.net.clj.dll
-a----      2016/02/09      17:21          4608 hello.net.exe
```

Those files are executables, provided you added the required Clojure and Microsoft DLLs as explained previously. On the command line, this time, no plugin required! This is pure .NET code:

```
.\hello.net.exe Osaka
; 5:34 pm
```

Sweet!

To be even more ready for the next step, we will add a specific method to be generated and exposed through the DLL. This is done at the :gen-