# # The Where it's snap app

Where it's snap will be our most-advanced app! It will have the following features:

+   - It will show a list of photos by their titles.
+   - It will offer the alternative of a list of tags, which when clicked takes you to a list of titles with that matching tag.
+   - It will allow the user to take photos with the device camera and assign a title as well as related tags.
+   - All of the data required (titles and tags) will be stored in a database. You will learn about databases in [Chapter 23, Using SQLite Databases in Our Apps], and we will add a database functionality to Where it's snap in [Chapter 24, Adding a Database to Where It's Snap].
+   - When the user taps on a photo's title in a list, he/she will see that photo and also be offered the option of showing a Google map of the location the photo was taken at.
+   - We will also see how to make this app multilingual (Spanish and English). Hola!

As with the Note To Self app, we will do things as correctly as possible and use string resources and Android naming conventions in our code.

We will build this app over the course of the next six chapters. As we explore and practice new features of the Android API, we will then utilize them by improving or adding features to Where it's snap.

The first thing we will do is prepare the project's resources and implement a navigation drawer. This will be the fundamental part of the UI for this project.

The project files that represent a runnable version of this app, as it is at the end of this chapter, can be found in the download bundle in the chapter 21/where its snap 1 folder.

Create a new project called where_its_snap and accept all the default settings as usual.

To handle Marshmallow runtime permissions, as discussed in [Chapter 11],