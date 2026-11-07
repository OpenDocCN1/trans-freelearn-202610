# TEXT

We will now explore how to use the TEXT field type and the relevant configuration values to adjust for the best possible phpMyAdmin behavior. First, we add to the book table a TEXT field called description.

There are three parameters that control the layout of the text area that will be displayed in Insert or Edit mode for the TEXT fields. First, the number of columns and rows for each field is defined by:

```
$cfg['TextareaCols'] = 40;
$cfg['TextareaRows'] = 7;
```

This gives (by default) the following space to work on a TEXT field.

![](bbox=[0.18, 0.35, 0.81, 0.4])

The settings impose only a visual limit on the text area, and a vertical scroll bar is created by the browser if necessary.

> Although MEDIUMTEXT, TEXT, and LONGTEXT columns can accommodate more than 32K of data, some browsers cannot always edit them with the text area — the mechanism offered by HTML. In fact, experimentation has convinced the phpMyAdmin development team to have the product display a warning message if the contents are larger than 32K. The message warns users that the contents may not be editable.

The last parameter has an impact for LONGTEXT fields. Setting $cfg['LongtextDoubleTextarea'] to TRUE, doubles the available editing space.

# BLOB (Binary Large Object) fields

BLOB fields are generally used to hold binary data (such as images and sounds), even though the MySQL documentation implies that TEXT fields could be used for this purpose. The MySQL 5.1 manual says: "In some cases, it may be desirable to store binary data such as media files in BLOB or TEXT columns". However, another phrase: "BLOB columns are treated as binary strings (byte strings)", seems to indicate that binary data should really be stored in BLOB fields. Thus, phpMyAdmin's intention is to work with BLOB fields to hold all binary data.

We will see in Chapter 16 that there are special mechanisms available to go further with BLOB fields, including being able to view some images directly from within phpMyAdmin. Furthermore, Chapter 17 covers BLOB streaming support.