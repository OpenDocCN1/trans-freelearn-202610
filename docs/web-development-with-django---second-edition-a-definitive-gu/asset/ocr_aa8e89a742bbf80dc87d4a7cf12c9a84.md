```
```html
placeholder="Enter your review"></textarea>
</p>
<p>
    <label for="id_rating">Rating</label><br/>
    <input id="id_rating" type="number" name="rating"
        placeholder="Rating 1-5">
</p>
<p>
    <button type="submit">Enter My Website</button>
</p>
</form>
```
```

When the template gets rendered, the template tag is interpolated, so the output HTML ends up like this (note that the inputs are still in the output – they have just been removed here for brevity):

```
```html
<form method="post" action=
"http://127.0.0.1:8000/books/4/reviews/">
    <input type="hidden" name="csrfmiddlewaretoken"
        value="tETZjLDUxEv1tiYqGCSbMQkhWiesHCnutxpt6mutHI6YH6
4F0nin5k2JW3B68IeJ">
    ...

</form>
```
```

Since this is a hidden field, the form on the page does not look any different from how it did before.

The CSRF token is unique to every visitor on the site. If an attacker were to copy the HTML from our site, they would get their own CSRF token, which would not match that of any other user, so Django would reject the form when it was posted by someone else.

CSRF tokens also change periodically. This limits how long the attacker would have to take advantage of a particular user and token combination. Even if they were able to get the CSRF token of a user that they were trying to exploit, they would have a short window of time to use it.

Now that you know what a CSRF token is used for, we will be adding it to our example form in the next exercise. Before we get to that, let’s have a quick refresh about accessing data in a request using QueryDict objects.