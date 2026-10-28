Now that we have learned about change lists and form customizations, we can put our knowledge into practice with a comprehensive activity.

# Activity 4.03 – customizing the Model admins

In our data model, the `Contributor` class is used to store data for book contributors; they can be authors, contributors, or editors. This activity focuses on modifying the `Contributor` class and adding a `ContributorAdmin` class to improve the user-friendliness of the admin app. At present, the `Contributor` change list defaults to a single column, `FirstNames`, based on the `__str__` method created in *Chapter 2, Models and Migrations*. We will investigate some alternative ways of representing this. These steps will help you complete the activity:

+   1. Edit `reviews/models.py` to add additional functionality to the `Contributor` model.
+   2. Add an `initialled_name` method to `Contributor` that takes no arguments (like the `Book.isbn13` method).
+   3. The `initialled_name` method will return a string containing `Contributor.last_names` followed by a comma and the initials of the given names. For example, for a `Contributor` object with `first_names` of `Jerome David` and `last_names` of `Salinger`, `initialled_name` will return `Salinger, JD`.
+   4. Replace the `__str__` method for `Contributor` with one that calls `initialled_name()`.