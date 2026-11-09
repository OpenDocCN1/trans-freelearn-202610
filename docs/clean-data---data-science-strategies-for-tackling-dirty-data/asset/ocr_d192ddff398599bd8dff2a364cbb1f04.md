If option 2 is chosen, typically, a double quote separator is then added to enclose data values. So, 129,000 on the first line would become "129,000".

# Escaping characters

What if the data itself has quotation marks in it? What if Sally's favorite color were listed and shown as light "Carolina" blue? Take a look:

```
First name,birth date,favorite color,salary
"Sally","1971-09-16","light "Carolina" blue","129,000"
"Manu","1984-11-03","red","159,960"
"Martin","1978-12-10","yellow","76,888"
```

Internal quotation marks will have to be escaped through the use of another special character, the backslash \:

```
First name,birth date,favorite color,salary
"Sally","1971-09-16","light \"Carolina\" blue","129,000"
"Manu","1984-11-03","red","159,960"
"Martin","1978-12-10","yellow","76,888"
```

> Or, we could try encapsulating with a single quote instead of double quotes, but then we might have issues with possessives such as "it's" or names such as O'Malley. There's always something!

Delimited files are very convenient in that they are easy to understand and easy to access in a simple text editor. However, as we have seen, they also require a bit of planning in advance in order to ensure that the data values are truly separated properly and everything is formatted the way it was intended.

If you find yourself the unlucky recipient of a file with delimiting errors such as the preceding ones, we'll give you some tricks and tips in Chapter 3, Workhorses of Clean Data – Spreadsheets and Text Editors, for how to clean them up.

# The JSON format

**JavaScript Object Notation (JSON)**, pronounced JAY-sahn, is one of the more popular formats for what is sometimes called semi-structured data. Contrary to the implications in its name, JSON is not dependent upon JavaScript to be useful. The name refers to the fact that it was designed to serialize JavaScript objects.