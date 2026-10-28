2. Once the library has been installed, the next step is to write the code that will fetch the total books read by the user as well as the books read by the user on a per-month basis. For this, create a new file named utils.py under the bookr application directory and add the required imports, which will be used to fetch the book-reading history of the user from the Review model of the reviews application:

```
import datetime

from django.db.models import Count
from reviews.models import Review
```

3. Next, create a new utility method named get_books_read_by_month() that takes in the username of the user for whom the reading history needs to be fetched.

4. Inside this method, we will query the Review model and return a dictionary of books read by the user on a per-month basis:

```
def get_books_read_by_month(username):
    """Get the books read by the user on per month basis.

    :param: str The username for which the books needs to be returned

    :return: dict of month wise books read
    """
    current_year = datetime.datetime.now().year

    books = Review.objects.filter(
        creator__username__contains=username,
        date_created__year=current_year)
        .values('date_created__month')
        .annotate(book_count=Count('book__title'))
    return books
```

Now, let’s examine the following query, which is responsible for fetching the results of books read this year every month:

```
Review.objects.filter(
    creator__username__contains=username,
    date_created__year=current_year)
    .values('date_created__month')
    .annotate(book_count=Count('book__title'))
```