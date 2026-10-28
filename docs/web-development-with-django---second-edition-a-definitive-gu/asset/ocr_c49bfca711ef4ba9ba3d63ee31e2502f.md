+   - The field class constructor takes a `choices` argument. The choices are provided as a tuple of two-element tuples. The first element in each sub-tuple is the value of the choice, while the second element is the text or description of the choice. For example, choices could be defined like this:

```
BOOK_CHOICES = (
    ("1", "Deep Learning with Keras"),
    ("2", "Web Development with Django"),
    ("3", "Brave New World"),
    ("4", "The Great Gatsby")
)
```

Note that you can use lists instead of tuples if you want (or a combination of the two). This can be useful if you want your choices to be mutable:

```
BOOK_CHOICES = (
    ["1", "Deep Learning with Keras"],
    ["2", "Web Development with Django"],
    ["3", "Brave New World"],
    ["4", "The Great Gatsby"]
]
```

+   - To implement an `optgroup`, we can nest the choices. To implement the choices as we did in our previous examples, we can use a structure like this:

```
BOOK_CHOICES = (
    (
        "Non-Fiction", (
            ("1", "Deep Learning with Keras"),
            ("2", "Web Development with Django")
        )
    ),
    (
        "Fiction", (
            ("3", "Brave New World"),
            ("4", "The Great Gatsby")
        )
    )
)
```