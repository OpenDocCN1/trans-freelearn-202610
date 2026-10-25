Proceed to select your file and advance to the import field mappings screen.
There you will click on the **Show Advanced Options** button located at the bottom
left. After clicking on it, you should see something similar to the following on the
bottom right of the screen:

![](bbox=[0.2312, 0.1508, 0.7969, 0.4062])

This area (as shown in the preceding image) permits us to select our match key
by selecting its related index. Notice that the options are limited to only those
indexes that are defined by SugarCRM, although it is possible to modify the
list through a minor customization.

Assuming you have applied a custom index to your database (as we have for
the **custom_c** field), you will need to make your SugarCRM system aware of
the new index.

To add it to your system's configuration, you must first use Notepad, vi, or other
text editor to create a file named `custom.php` with content similar to the following:

```php
<?php
$dictionary['Account']['indices'][] = array('name'
=>'idx_custom', 'type'=>'index',
'fields'=>array('custom_c'));
?>
```

Make a note of the **Account** reference as this specifies the module to be affected.
Notice that it is specified in the singular form. Users will often make the mistake
of specifying the module name in its plural form which will cause these types of
customizations to fail. All module references must be in singular form.