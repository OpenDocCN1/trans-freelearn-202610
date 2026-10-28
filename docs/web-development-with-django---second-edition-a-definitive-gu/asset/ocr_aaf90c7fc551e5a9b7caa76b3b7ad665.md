```
first_names = models.CharField(
    max_length=50,
    help_text="The contributor's first name or names.")
last_names = models.CharField(
    max_length=50,
    help_text="The contributor's last name or names.")
email = models.EmailField(
    help_text="The contact email for the contributor.")

def str (self):
    return self.first_names
```

Similarly, the string representation of book is title, so the returned value is self.title, with self referring to the Book object. The string representation of Contributor is the first name of the contributor; hence self.first_names is returned. Here, self refers to the Contributor object. Next, we will look at migrating the reviews app.

# Migrating the reviews app

Since we have the entire model file ready, let’s now migrate the models into the database, similar to what we did before with the installed apps. Since the reviews app has a set of models created by us, it is important to create the migration scripts before running the migration. Migration scripts help in identifying any changes to the models and will propagate these changes into the database while running the migration. Follow these steps to create migration scripts and then migrate the models into the database:

+   1. Execute the following command to create the migration scripts:

```
python manage.py makemigrations reviews
```

You should get an output similar to this:

```
reviews/migrations/0002_auto_20191007_0112.py
  - Create model Book
  - Create model Contributor
  - Create model Review
  - Create model BookContributor
  - Add field contributors to book
  - Add field publisher to book
```

Migration scripts will be created in a folder named migrations in the application folder.

+   2. Next, migrate all the models into the database using the migrate command:

```
python manage.py migrate reviews
```