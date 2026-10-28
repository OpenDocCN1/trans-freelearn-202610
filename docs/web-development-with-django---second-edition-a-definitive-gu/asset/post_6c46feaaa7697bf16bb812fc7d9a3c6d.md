
3. Use ADD GROUP + in the top right-hand corner to add a new group:

![](img/4840a43efd335abd10b93708225692d6_183_0.png)

Figure 4.15 – Adding a new group

4. Name the group Help Desk User and give it the following permissions, as shown in Figure 4.16:

- Can view log entry
- Can view permission
- Can change user
- Can view user

![](img/4840a43efd335abd10b93708225692d6_183_1.png)

Figure 4.16 – Selecting permissions

5. Now, navigate to Home | AUTHENTICATION AND AUTHORIZATION | Users and click the link of the user with the first name carol:

Select user to change

Figure 4.19 – Clicking on the username carol

6. Scroll down to the Permissions fields set and select the Staff status checkbox. This is required for Carol to be able to log in to the admin app:

Figure 4.20 – Clicking the Staff status checkbox

7. Add Carol to the **Help Desk User** group that we created in the previous steps by selecting it from the **Available groups** selection box (refer to Figure 4.20) and clicking the right arrow to shift it into her list of **Chosen groups** (as shown in Figure 4.21). Note that unless you do this, Carol won’t be able to log in to the admin interface using her credentials:

![](img/4840a43efd335abd10b93708225692d6_186_0.png)

Figure 4.21 – Shifting the Help Desk User group into the list of Chosen groups for Carol

8. Let’s test whether what we’ve done up till now has yielded the right outcome. To do this, log out of the admin site and log in again as Carol. Upon logging out, you should see the following on your screen:

![](img/4840a43efd335abd10b93708225692d6_186_1.png)

Figure 4.22 – The Logged out screen

**Note**
If you don’t recall the password that you initially gave her, you can change the password in the command line by typing `python3 manage.py changepassword carol`.

Upon successful login, on the admin dashboard, you can see that there is no link to Groups:

![](img/4840a43efd335abd10b93708225692d6_187_0.png)

Figure 4.23 – The admin dashboard

As we did not assign any group permissions, not even auth | group | Can view group, to the Help Desk User group, when Carol logs in, the Groups admin interface is not available to her. Similarly, when you navigate to Home | AUTHENTICATION AND AUTHORIZATION | Users and click a user link, you will see that there are no options to edit or delete the user. This is because of the permissions that were granted to the Help Desk User group, of which Carol is a member. The members of the group can view and edit users but cannot add or delete any user.

In this exercise, we learned how to grant a certain amount of administrative privileges to users of our Django app.

Now that we have an understanding of how the admin app works with the AUTHENTICATION AND AUTHORIZATION models for Users and Groups, we can turn our attention to registering the models that we have developed in the reviews app.

## Registering models with the admin app

The Django admin app produces workable CRUD interfaces to Django objects based on the characteristics of the models, with a minimal amount of coding. In this section, we will first look at how to register the models with the admin app and how elements of the interface, such as change lists and change pages, are derived from the model properties. We will conclude with an important exercise that demonstrates how foreign key settings in the model determine the behavior of the object deletion process.

## Registering the reviews model

Let's say that Carol is tasked with improving the **Reviews** section in Bookr; that is, only the most relevant and comprehensive reviews should be shown, and duplicate or spam entries should be removed. For this, she will need access to the `reviews` model. As we have seen previously with our investigation of groups and users, the admin app already contains admin pages for the models from the Authentication and Authorization app, but it does not yet reference the models in our Reviews app. To make the admin app aware of the models, we need to explicitly register them with the admin app. Fortunately, we don't need to modify the admin app's code to do so, as we can instead import the admin app into our project and use its API to register our models. This has already been done in the Authentication and Authorization app, so let's try it with our Reviews app. Our aim is to be able to use the admin app to edit the data in our `reviews` model.

Take a look at the `reviews/admin.py` file. It is a placeholder file that was generated with the `startapp` subcommand that we used in *Chapter 1, An Introduction to Django*, and currently contains these lines:

```
from django.contrib import admin

# Register your models here.
```

Now we can try to expand this. To make the admin app aware of our models, we can modify the `reviews/admin.py` file and import the models. Then we can register the models with the `AdminSite` object, `admin.site`. The `AdminSite` object contains the instance of the Django admin application (later, we will learn how to subclass this `AdminSite` and override many of its properties). Then, `reviews/admin.py` will look as follows:

```
from django.contrib import admin
from reviews.models import (Publisher, Contributor,
                            Book, BookContributor, Review)

# Register your models here.
admin.site.register(Publisher)
admin.site.register(Contributor)
admin.site.register(Book)
admin.site.register(BookContributor)
admin.site.register(Review)
```

The `admin.site.register` method makes the models available to the admin app by adding it to a registry of classes contained in `admin.site._registry`. If we chose not to make a model accessible through the admin interface, we would simply not register it. When you reload http://127.0.0.1:8000/admin/ in your browser, you will see the following on the admin app landing page:

## Site administration

![](img/4840a43efd335abd10b93708225692d6_189_0.png)

Figure 4.24 – Admin app landing page

Note the change in the appearance of the admin page after the reviews model has been imported.

With the five models of the Reviews app registered with the admin app, we can examine some of the default interfaces that it has created for us.

## Change lists

We now have change lists populated for our models. If we click the Publishers link, we will be taken to http://127.0.0.1:8000/admin/reviews/publisher and see a change list containing links to the publishers. These links are designated by the id field of the Publisher objects. If your database has been populated with the script in Chapter 3, URL Mapping, Views, and Templates, you will see a list with seven publishers, as shown in Figure 4.25.

> **Note**
> Depending on the state of your database and based on the activities you have completed, the object IDs, URLs, and links in these examples may be numbered differently from those listed here.

# Select publisher to change

Action: ______ Go 0 of 9 selected

- PUBLISHER
- Pocket Books
- CreateSpace Independent Publishing Platform
- Houghton Mifflin Harcourt
- Simon and Schuster
- Bay Back Books
- Scribner
- Penguin Classics
- Harper Collins
- Packt Publishing
9 publishers

Figure 4.25 – The Select publisher to change list

We can follow one of the links in the change list and examine the Change publisher page.

## The Change publisher page

The Change publisher page at http://127.0.0.1:8000/admin/reviews/publisher/1 contains what we might expect (see Figure 4.26). There is a form for editing the publisher's details. These details have been derived from the reviews.models.Publisher class:

# Change publisher

![](img/4840a43efd335abd10b93708225692d6_191_0.png)

Figure 4.26 – The Change publisher page

If we had clicked the **ADD PUBLISHER +** button, the admin app would have returned a similar form for adding a publisher. The beauty of the admin app is that it gives us all of this CRUD functionality with just one line of coding – `admin.site.register(Publisher)` – using the definition of the `reviews.models.Publisher` attributes as a schema for the page content:

```
```python
class Publisher(models.Model):
    """A company that publishes books."""
    name = models.CharField(max_length=50,
        help_text="The name of the Publisher.")
    website = models.URLField(
        help_text="The Publisher's website.")
    email = models.EmailField(
        help_text="The Publisher's email address.")
```
```

The publisher **Name** field is constrained to 50 characters as specified in the model. The help text that appears in gray below each field is derived from the `help_text` attributes specified on the model. We can see that `models.CharField`, `models.URLField`, and `models.EmailField` are rendered in HTML as input elements of the text, url, and email types, respectively.
