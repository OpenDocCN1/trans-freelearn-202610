# Handling the SQLite database

Create a new class called DataManager. We need an SQLiteDatabase object and
a whole bunch of final string members for the table and column names.

All the code that we will add first to our DataManager is straightforward, but it is
worth pointing out that we will actually have two tables in this database. One
called wis_table_photos will have a column for each of the member variables
of our Photo class.

Another called wis_table_tags will have just two columns, autoincrement
_ID and a tag column.

The wis_table_photos table will have a row of data for each and every photo
the user takes. The tags column will only have a row for each new/unique tag
that the user enters. So, if the user has more than one photo with the "Barbados"
or "2016" tag, it will only appear once in the wis_table_tags table but will
appear with each and every photo that uses it in the wis_table_photos table.

This way, it will be possible to show the user a list of all the unique tags they
have added, and when they tap on a tag, it will present them with a list of all the
photos that have been tagged with this tag.

It would probably help clarify the usage of all the strings in the code that follow
to have a spreadsheet eye view to help visualize our two tables:

| _id | image_title | image_uri | tag1 | tag2 | tag3 |
|---|---|---|---|---|---|
| 1 | Dave in the pool | /external/ima | 2016 | Barbados | Fun |
| 2 | Sarah on the beach | /external/ima | 2015 | Barbados | Pretty |
| 3 | My new car | /external/ima | 2016 | UK | Sad |

In the previous image, you can see that the wis_table_photos table has all the