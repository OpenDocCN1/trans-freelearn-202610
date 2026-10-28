It is a required field, and it has a text type and maxlength of 50, as defined by the `max_length` parameter in the model definition:

```
name = models.CharField(max_length=50,
    help_text="The name of the Publisher.")
```

Similarly, we can see the website and email being defined in the model as URLField and EmailField are rendered in HTML as input elements of the url and email types, respectively:

```
<input type="url" name="website"
    value="https://www.packtpub.com/"
    class="vURLField" maxlength="200" required=""
    id="id_website">
<input type="email" name="email" value="info@packtpub.com"
    class="vTextField" maxlength="254"
    required="" id="id_email">
```

We have learned that the Django admin app derives sensible HTML representations of Django models based on the model definitions that we have provided.

Now let’s examine the Book change page.

# The Book change page

Similarly, there is a change page that can be reached by selecting **Books** from the **Site administration** page and then selecting a specific book from the change list:

![](bbox=[0.206, 0.571, 0.795, 0.871])

Figure 4.28 – Selecting Books from the Site administration page