
This exercise has enhanced the profile page by adding a list of recently viewed books. Now, when you visit the profile link at `http://127.0.0.1:8000/accounts/profile/`, you will see this page:

![](img/4840a43efd335abd10b93708225692d6_488_0.png)

Figure 9.18 – Recently viewed books

We can use the `sessioninfo` management command that we developed in *Exercise 9.05 – examining the session key*, to examine the user’s session once this feature is implemented. It can be called on the command line by passing the session data as an argument:

```bash
python manage.py sessioninfo
```

We can see that the book IDs and titles are listed in the `viewed_books` key. Remember that the encoded data is obtained by querying the `django_session` table in the SQLite database:

```text
> python manage.py sessioninfo
Session Key: mjbbpj6e6y0qayh1mj6sgw27kfugq1qc User: 4 alice alice.white@example.com
{'_auth_user_backend': 'django.contrib.auth.backends.ModelBackend',
 '_auth_user_hash': '384340e0724b154062dca2d2c94a9fc94380dc323b9e1e433aff3aef799b99a3e',
 '_auth_user_id': '4',
 'viewed_books': [[17, 'Paul Clifford'],
                  [1, 'Advanced Deep Learning with Keras'],
                  [13, 'Fahrenheit 451'],
                  [6, 'Hands-On Reinforcement Learning with Python'],
                  [4, 'Deep Reinforcement Learning Hands-On']]}
```

Figure 9.19 – The viewed books are stored in the session data

In this exercise, we used Django’s session mechanism to store ephemeral information about user interactions with the Django project. We have learned how this information can be retrieved from the user session and displayed in a view that informs users about their recent activity.

## Activity 9.02 – using session storage for the Book Search page

Sessions are a useful way to store short-lived information that assists in maintaining a stateful experience on a site. Users frequently revisit pages such as search forms, and it would be convenient to store their most recently used form settings when they return to those pages. In Chapter 3, URL Mapping, Views, and Templates, we developed a book search feature for the bookr project. The Book Search page has two options for Search in – Title and Contributor. Currently, each time the page is visited, it defaults to Title:

![](img/4840a43efd335abd10b93708225692d6_489_0.png)

Figure 9.20 – The Search and Search in fields of the Book Search form

In this activity, you will use session storage so that when the Book Search page, /book-search, is visited, it will default to the most recently used search option. You will also add a third infocell to the profile page that contains a list of links to the most recently used search terms. Follow these steps to complete this activity:

1. Edit the book_search view and retrieve search_history from the session.
2. When the form has received valid input and a user is logged in, append the search option and search text to the session's search history list.

   If the form hasn't been filled (for example, when the page is first visited), render the form with the previously used Search in option selected – that is, either Title or Contributor (Figure 9.21):

   ![](img/4840a43efd335abd10b93708225692d6_489_1.png)

   Figure 9.21 – Selecting Contributor in the Search field

3. In the profile template, include an additional `infocell` division for Search History.
4. List the search history as a series of links to the book search page. The links will take this form: `/book-search?search=Python&search_in=title`.

This activity will challenge you to apply session data to solve a usability issue in a web form. This approach will have applicability in many real-world situations and will give you some idea of the use of sessions in creating a stateful web experience. After completing this activity, the profile page will contain the third infocell, as shown in Figure 9.22:

![](img/4840a43efd335abd10b93708225692d6_490_0.png)

Figure 9.22 – The profile page with the Search History infocell

> **Note**
> You can find the solution for this activity on https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/ActivitySolutions.

## Summary

In this chapter, we examined Django’s middleware implementation of authentication and sessions. We also learned how to incorporate authentication and permission logic into views and templates. Now, we can set permissions on specific pages and limit their access to authenticated users. We also examined how to store data in a user’s session and render it on subsequent pages.

With that, you have the skills to customize a Django project to deliver a personalized web experience. You can limit the content to authenticated or privileged users and you can personalize a user’s experience based on their prior interactions. In the next chapter, we will revisit the Admin app and learn about some advanced techniques we can use to customize our user model and apply fine-grained changes to the admin interface for our models.

## Advanced Django Admin and Customizations

Let’s say we want to customize the front page of a large organization’s admin site. We want to show the health of the different systems in the organization and see any high-priority alerts that are active. If this were an internal website built on top of Django, we would need to customize it. Adding these kinds of functionalities will require the developers in the IT team to customize the default admin panel and create their own custom AdminSite module, which will render a different index page in comparison to what is provided by the default admin site. Fortunately, Django makes these kinds of customizations easy.

In this chapter, we will look at how we can leverage Django’s framework and its extensibility to customize Django’s default admin interface (as shown in *Figure 10.1*). We’ll not just learn how to make the interface more personal; we will also learn how we can control the different aspects of the admin site to make Django load a custom admin site, instead of the one that ships with the default framework. Such customization can come in handy when we want to introduce features into the admin site that are not present by default:

![](img/4840a43efd335abd10b93708225692d6_493_0.png)

Figure 10.1 – Default Django administration panel interface

This chapter builds upon the skills we practiced in *Chapter 4, Introduction to Django Admin*. Just to recap, we learned how to use the Django admin site to take control of the administration and authorization for our Bookr app. We also learned how to register models to read and edit their contents and also to customize Django’s admin interface using the `admin.site` properties.

The following topics are covered in this chapter:

- Customizing the admin site
- Adding views to the admin site

Now, let’s expand our knowledge further by taking a look at how we can start customizing the admin site by utilizing Django’s `AdminSite` module to add powerful new functionalities to the admin portal of our web application.

### Technical requirements

The complete code files for this chapter can be found at `https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter10`

### Customizing the admin site

Django, as a web framework, provides a lot of customization options for building web applications. We will be using this same freedom provided by Django when we work on building the admin application for our project.

In *Chapter 4, Introduction to Django Admin*, we looked at how we can use the `admin.site` properties to customize the elements of Django’s admin interface. However, what if we require more control over how our admin site behaves? For example, what if we wanted to show a custom template for the login page or the logout page when the user comes to the Bookr admin panel? In this case, the `admin.site` properties provided might not be enough, and we will need to build customizations that can extend the default admin site’s behavior. Luckily, this can be easily achieved by extending the `AdminSite` class from Django’s admin module. However, before we jump into building our admin site, let’s first understand how Django discovers admin files and how we can use this admin file discovery mechanism to build a new app inside Django that will act as our admin site app.

### Discovering admin files in Django

When we build applications in our Django project, we use the `admin.py` file frequently to register our models or create `ModelAdmin` classes that customize our interactions with the models inside the admin interface. These `admin.py` files store and provide this information to our project’s admin interface. The discovery of these files is effected automatically by Django once we add `django.contrib.admin` to the `INSTALLED_APPS` section inside our `settings.py` file:
