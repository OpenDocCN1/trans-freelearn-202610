
```html
<li class="producerlist">
    <div class="name">
        algae
    </div>
    <div class="number">
        100000
    </div>
</li>
</ul>
</div>
</body>
</html>
```

The `replace_with()` method can also be used to replace a tag completely.

### The `wrap()` and `unwrap()` methods

The `wrap()` method is used to wrap a tag or string with another tag that we pass. For example, we can wrap the entire contents in the `li` tag with another `<div>` tag in our following example:

```python
li_tags = soup.find_all("li")
for li in li_tags:
    new_divtag = soup.new_tag("div")
    li.wrap(new_divtag)
print(soup.prettify())
#output
<html>
<body>
    <div class="ecopyramid">
        <ul id="producers">
            <div>
                <li class="producerlist">
                    <div class="name">
                        phytoplankton
                    </div>
                    <div class="number">
                        100000
                    </div>
                    <div class="ecosystem">
                        soil
                    </div>
                </li>
            </div>
            <div>
                <li class="producerlist">
                    <div class="name">
                        algae
                    </div>
                    <div class="number">
                        100000
                    </div>
                </li>
            </div>
        </ul>
    </div>
</body>
</html>
```

From the preceding output, it is clear that we wrapped the `li` tag with another `div` tag.

The `unwrap()` method does the opposite of `wrap()` and is used to unwrap the contents as follows:

```python
soup = BeautifulSoup(html_markup,"lxml")
li_tags = soup.find_all("li")
for li in li_tags:
    li.div.unwrap()
print(soup.prettify())
#output
<html>
<body>
    <div class="ecopyramid">
        <ul id="producers">
            <li class="producerlist">
                plants
                <div class="number">
                    100000
                </div>
                <div class="ecosystem">
                    soil
                </div>
            </li>
        </div>
        <div>
            <li class="producerlist">
                algae
                <div class="number">
                    100000
                </div>
            </li>
        </div>
    </ul>
</div>
</body>
</html>
```

Here, the first `div` tag will be unwrapped, that is, the `div` tag with `class="name"`.

> The `unwrap()` method, such as `replace_with()`, will return the tag that was replaced.

## Quick reference

- Modifying the Tag name:
  The following code line modifies the name property:
  - `tag.name = "newvalue"`: This line of code modifies the Tag name as newvalue
- Modifying the Tag attribute:
  The following code lines alter the attributes:
  - `tag["attr"] = "newvalue"`: This line of code modifies the Tag attribute
  - `del tag["attr"]`: This line of code deletes the Tag attribute
- Adding new tags:
  The following code lines correspond to the addition of content:
  - `newtag = soup.new_tag('tagname')`: This line of code creates newtag
  - `oldtag.append(newtag)`: This line of code appends newtag to oldtag.contents
  - `oldtag.insert(0,newtag)`: This line of code inserts newtag at the index 0 of oldtag.contents
- Modifying the string contents:
  The following code lines are used to modify the string content:
  - `tag.string = "helloworld"`: This line of code modifies `tag.string`
  - `tag.append("helloworld")`: This line of code appends "helloworld" to the existing `tag.string`
  - `newstring = soup.new_string("helloworld")`: This line of code creates a new `NavigableString` object
  - `tag.insert(0,newstring)`: This line of code inserts `newstring` at the index `0` of `tag.string`
- Deleting the existing tags:
  The following code lines help to remove the existing tag attributes:
  - `tag.decompose()`: This line of code removes a tag and its children
  - `tag.extract()`: This line of code removes and returns a tag or string
  - `tag.clear()`: This line of code removes children
- Special functions:
  The following are the special functions used to add or alter tags:
  - `oldtag.insert_after(newtag)`: This function inserts `newtag` after `oldtag`
  - `oldtag.insert_before(newtag)`: This function inserts `newtag` before `oldtag`
  - `oldtag.replace_with(newtag)`: This function replaces `oldtag` with `newtag`
  - `oldtag.wrap(newtag)`: This function wraps `oldtag` with `newtag`
  - `oldtag.unwrap(newtag)`: This function unwraps `newtag` within `oldtag`

## Encoding Support in Beautiful Soup

All web pages will have an encoding associated with it. Modern websites have different encodings such as UTF-8 and Latin-1. Nowadays, UTF-8 is the encoding standard used in websites. So, while dealing with the scraping of such pages, it is important that the scraper should also be capable of understanding those encodings. Otherwise, the user will see certain characters in the web browser whereas the result you would get after using a scraper would be gibberish characters. For example, consider a sample web content from Wikipedia where we are able to see the Spanish character ñ.

### Alphabet

The Spanish language is written using the Spanish alphabet, which is the Latin alphabet with one additional letter, ñ, for a total of 27 letters.

| Letter | A | B | C | D | E | F | G | H | I | J | K | L | M | N | Ñ | O | P | Q | R | S | T | U | V | W | X | Y | Z |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Name | a | be, be larga, be alta | ce | de | e | efe | ge | hache | i | jota | ka | ele | eme | ene | eñe | o | pe | cu | erre | ese | te | u | ve, uve, ve corta, ve baja | uve doble, doble ve, doble uve | equis | i griega, ye | zeta |
| IPA | /a/ | /b/ | /k/, /θ/ | /d/ | /e/ | /f/ | /g/, /x/ | silent | /i/ | /x/ | /k/ | /l/ | /m/ | /n/ | /ɲ/ | /o/ | /p/ | /k/ | /r/, /ɾ/ | /s/ | /t/ | /u/ | /b/ | /ɡw/, /β/ | /ks/, /ɣ/, /s/ | /ʝ/, /ʎ/ | /θ/ |

If we run the same content through a scraper with no support for the previous encoding used by the website, we might end up with the following content:

```
The Spanish language is written using the Spanish alphabet, which
is the Latin alphabet with one additional letter,
e&#21336;e (&#21336;), for a total of 27 letters.
```

We see the Spanish character ñ is replaced with gibberish characters. So it is important that a scraper should support different encodings. Beautiful Soup handles these encodings pretty neatly. In this chapter, we will see the encoding support in Beautiful Soup.

## Encoding in Beautiful Soup

As already explained, every HTML/XML document will be written in a specific character set encoding, for example, UTF-8 and Latin-1. In an HTML page, this is represented using the meta tag as shown in the following example:

```
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
```

Beautiful Soup uses the UnicodeDammit library to automatically detect the encoding of the document. Beautiful Soup converts the content to Unicode while creating soup objects from the document.

> Unicode is a character set, which is a list of characters with unique numbers. For example, in the Unicode character set, the number for the character B is 42. UTF-8 encoding is an algorithm that is used to convert these numbers into a binary representation.

In the previous example, Beautiful Soup converts the document to Unicode.

```
html_markup = """<p> The Spanish language is written using the
Spanish alphabet, which is the Latin alphabet with one
additional letter, eñe (ñ), for a total of 27 letters.</p>
"""

soup = BeautifulSoup(html_markup,"lxml")
print(soup.p.string)

#output
The Spanish language is written using the Spanish alphabet, which
is the Latin alphabet with one additional letter, eñe (ñ), for
a total of 27 letters.
```

From the previous output, we can see that there is a difference in the additional letter part (e単e (単)) because there is a gibberish character instead of the actual representation. This is due to the wrong interpretation of the original encoding in the document by UnicodeDammit.

### Understanding the original encoding of the HTML document

The previous HTML content was originally of UTF-8 encoding. But Beautiful Soup, while generating the soup object, detects encoding using the UnicodeDammit library and we can get hold of this original encoding using the attribute `original_encoding`.

The `soup.original_encoding` will give us the original encoding, which is `euc-jp` in the previous case. This is wrong because UnicodeDammit detected the encoding as `euc-jp` instead of `utf-8`.

### Specifying the encoding of the HTML document

The UnicodeDammit library detects the encoding after a careful search of the entire document. This is time consuming and there are cases where the UnicodeDammit library detects the encoding as wrong, as observed previously. This wrong guess happens mostly when the content is very short and there are similarities between the encodings. We can avoid these if we know the encoding of the HTML document upfront. We can pass the encoding to the BeautifulSoup constructor so that the excess time consumption and wrong guesses can be avoided. The encoding is specified as a part of the constructor in the `from_encoding` parameter. So in the previous case, we can specify the encoding as `utf-8`.

```
soup = BeautifulSoup(html_markup,"lxml",from_encoding="utf-8")
print(soup.prettify())
```

```
#output
<html>
  <body>
    <p>
      The Spanish language is written using the Spanish alphabet,
      which is the Latin alphabet with one additional letter,
      eñe (ñ), for a total of 27 letters.
    </p>
  </body>
</html>
```
