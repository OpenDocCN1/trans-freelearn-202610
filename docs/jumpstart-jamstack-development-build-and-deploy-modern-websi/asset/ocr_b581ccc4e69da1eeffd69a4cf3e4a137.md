Now that we have covered some basic queries, let's go through more advanced GROQ queries.

# Advanced GROQ

In this section, we will see how to get fields from relations, how to create our own formatted response, and finally, we will explore some built-in functions.

# Getting events by venues

On a venue page, we want to select all events at that particular venue:

```
```groq
*[_type == "event" && venue->name == "Will's Pub"]{
  name,
  venue->{name}
}
```
```

The result will be as follows:

```
```json
[
  {
    "name": "The Appleseed Cast",
    "venue": {
      "name": "Will's Pub"
    }
  },
  {
    "name": "Friday Drinks",
    "venue": {
      "name": "Will's Pub"
    }
  }
]
```
```

Not that the venue, Will's Pub, shows results for two events.