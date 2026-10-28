While columns that are derived from model attributes derive their column headers and properties from the field attributes, we can specify properties of display columns that are computed fields using the display decorator.

# The display decorator

When using a callable in the `list_display`, as in the cases of `initialled_name` and `isbn13`, we can use the `admin.display` decorator to specify the column name that will appear in the header of the change list using the `description` argument. We can also use it to get around the limitation of calculated fields not being sortable by specifying `ordering` on the callable. The `empty_value` argument can be used to specify how a None value or empty string is displayed. The default empty_value display is a single dash character:

```
@admin.display(
    ordering='isbn',
    description='ISBN-13',
    empty_value='-/-'
)
def isbn13(self, obj):
    """ '9780316769174' => '978-0-31-676917-4' """
    return "{}-{}-{}-{}-{}".format(obj.isbn[0:3],
                                   obj.isbn[3:4], obj.isbn[4:6], obj.isbn[6:12],
                                   obj.isbn[12:13])
```

The boolean argument to admin.display can be used to flag a value to be represented in Boolean form:

```
@admin.display(
    boolean=True,
    description='Has ISBN',
)
def has_isbn(self, obj):
    """ '9780316769174' => True """
    return bool(obj.isbn)
```