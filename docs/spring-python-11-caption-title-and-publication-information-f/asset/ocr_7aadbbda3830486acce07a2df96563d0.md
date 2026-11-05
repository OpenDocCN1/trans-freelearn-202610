3. Now let's launch the script:

![](bbox=[0.25, 0.17, 0.75, 0.4])

In this script, we turned on some of Spring Python's built in logging, so we could see some of parts involved with starting up remoting. We can see two different Pyro daemon threads being launched, one for each of the ports. And then we see two instances of our Service code being registered, one with each thread.

We now have two copies of our service ready and waiting to be called.

# Creating a round-robin dispatcher

There are many different ways to code a dispatcher. For our example, let's pick a simple round robin algorithm where we cycle through a fixed list.

Let's create an application context that includes a round robin dispatcher that acts like our service, fed with a list of PyroProxyFactory elements.

```
from springpython.config import *
from springpython.remoting.pyro import *

class RoundRobinDispatcher(object):
    def __init__(self, proxies):
        self.proxies = proxies
        self.counter = 0

    def happy_birthday(self, parm):
        self.counter += 1
        proxy = self.proxies[self.counter % 2]
        print "Calling %s" % proxy
```