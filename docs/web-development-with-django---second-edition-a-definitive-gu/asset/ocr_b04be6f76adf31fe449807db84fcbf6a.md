For example, we have two tables, namely Book and Contributor, where we need to store the information on the type of contributor for the book, such as Author, Co-Author, or Editor. Then the type of contributor is stored in an intermediary table called BookContributor. Here is how the BookContributor table/model looks. Make sure you include this model in reviews/models.py:

```
class BookContributor(models.Model):
    class ContributionRole(models.TextChoices):
        AUTHOR = "AUTHOR", "Author"
        CO_AUTHOR = "CO_AUTHOR", "Co-Author"
        EDITOR = "EDITOR", "Editor"

    book = models.ForeignKey(
        Book, on_delete=models.CASCADE)
    contributor = models.ForeignKey(
        Contributor, on_delete=models.CASCADE)
    role = models.CharField(
        verbose_name="The role this contributor had in the \
        book.", choices=ContributionRole.choices,
        max_length=20)
```

> **Note**
> The complete models.py file can be viewed here: https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/blob/main/Chapter02/final/bookr/reviews/models.py.

An intermediary table such as BookContributor establishes relationships by using foreign keys to both the Book and Contributor tables. It can also have extra fields that can store information about the relationship the BookContributor model has with the following fields:

+   - book: This is a foreign key to the Book model. As we saw previously, on_delete=models.CASCADE will delete an entry from the relationship table when the relevant book is deleted from the application.
+   - Contributor: Again, this is a foreign key to the Contributor model/table. This is also defined as CASCADE upon deletion.
+   - role: This is the field of the intermediary model, which stores the extra information about the relationship between Book and Contributor.
+   - class ContributionRole(models.TextChoices): This can be used to define a set of choices by creating a subclass of models.TextChoices. For example, ContributionRole is a subclass created out of TextChoices, which is used by the roles field to define Author, Co-Author, and Editor as a set of choices.