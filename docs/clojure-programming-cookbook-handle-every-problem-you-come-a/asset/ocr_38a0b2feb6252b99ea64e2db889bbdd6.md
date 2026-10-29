before this book came out so that is why we stick to a slightly older version. For our purposes here, that will not be a problem.

You are now ready to execute your first bits of Clojure code from your C# project.

Let's stick in some usual interop code in the core of the main application:

```
using System;
using clojure.lang;
using System.Threading;
using clojure.clr.api;

namespace firstclojure
{
class Program
{
    {
        static void Main(string[] args)
        {
            IFn load = clojure.clr.api.Clojure.var("clojure.core",
"load-string");
            Console.WriteLine(Convert.ToString(load.invoke("(+ 1 1)")));
            Thread.Sleep(1000);
        }
    }
}
```

Executing the preceding, and assuming the installation went fine, the following will show up:

2

This means that the Clojure code has effectively been compiled using the load-string method, and has been eval-ed. We're sleeping a bit more in the end, so we have time to see the message. It is worth noting that the Objects explorer window shows a reference to the core Clojure libraries: