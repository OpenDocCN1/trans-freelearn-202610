The following code shows the use of `next_element`. While traversing along a DOM (normally from top to bottom), various elements can be found with `next_element`:

```
soup.find('p','story').next_element
# 'Once upon a time there were three little sisters; and
#  their names were\n '

soup.find('p','story').next_element.next_element
# <a class="sister" href="http://example.com/elsie"
#  id="link1">Elsie</a>
```

Let us understand the details of the preceding code:

+   - `find('p','story').next_element`: Searches for a `<p>` element for the story value in class, and points to the next element. Here, the next element after `<p class="story">` is the text itself, as seen in the preceding code output.
+   - `find('p','story').next_element.next_element`: Points to the `<a>` child element with the text `Elsie`.

`previous_element` is similar to `next_element`, but the direction of traversing is reversed.

The following code shows the use of `previous_element`, which can be extended or chained:

```
soup.find('b').previous_element
# <p class="title"><b>The Dormouse's story</b></p>

soup.find('li',{'data-id':'45732'}).previous_element
    .previous_element
# 'Alex Trevelyan, 006: Agent turned terrorist leader;
#  James\'... in "Goldeneye".'
```

Regarding the preceding code, let us understand a few details:

+   - `find('b').previous_element`: There’s only one `<b>` element in the HTML content inside `<p>`, so it returns the element at `previous_element` or `<p class="title">`
+   - `find('li',{'data-id':'45732'}).previous_element.previous_element`: With `<li data-id="45732">`, we find that the previous element is `<li data-id="97865">`, hence we get the text

The `next_element` and `previous_element` attributes can be chained, as seen in the preceding code.

We have explored a lot regarding searching and traversing so far in this section. The next section demonstrates performing iteration.