# Download the example code files

The code bundle for the book is hosted on GitHub at https://github.com/PacktPublishing/ High-Performance-with-MongoDB/.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing. Check them out!

# Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: https://packt.link/gbp/9781837022632.

# Conventions used

There are a number of text conventions used throughout this book.

CodeInText: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and X (Twitter) handles. For example: “$project and $addFields serve different purposes and have different performance implications.”

A block of code is set as follows:

```
```
# A simple document representing a user
user = {
    "name": "Jane Doe",
    "age": 30,
    "email": "janedoe@example.com",
    "address": {
        "street": "123 Main St",
        "city": "Anytown",
        "state": "CA",
        "zip": "12345"
    },
    "interests": ["hiking", "photography", "travel"]
}
```
```

Any command-line input or output is written as follows:

```
```
db.adminCommand({ serverStatus: 1 })
```
```