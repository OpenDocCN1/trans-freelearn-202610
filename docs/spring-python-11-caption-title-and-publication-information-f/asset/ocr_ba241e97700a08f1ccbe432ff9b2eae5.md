7. One way to let the customers search is against all the data in the flight record

```
def __str__(self):
    return "%s %s %s" % \
        (self.flight, self.departure, self.arrival)
```

With all these changes, we can now do a simple flight search.

![](bbox=[0.2, 0.25, 0.79, 0.52])

What are some other search options you would consider to make this more sophisticated?

# Moving from sample Python data to real Java data

So far, we are looking at a couple rows of sample data. A more realistic scenario would be tapping some type of a live system, listing real-time flights. What would you do if the data was stored in a backend database and a service was already coded in Java?

+   - Would you attempt to bypass the Java system and talk directly to the database? What risks are involved with this approach?
+   - Would you ditch the Python front end we just built and instead embark on a Java-based portal? What if this had been several months of effort?