In our first example, we will change the default behavior of the name field within the cases module from text to drop-down. This will allow us to limit the values that are assigned to the field to only those defined within a list. This is a technique that helps enforce uniformity and improves metrics.

Note that creating a drop-down field requires the specification of a drop-down list containing the values from which a user can choose. We will assume that the drop-down list to be used for this exercise is one named `cases_names` which was previously created using the Dropdown Editor tool.

To modify the default behavior of the name field and convert it to a drop-down, create a PHP file with the following contents:

```
```php
<?php
$dictionary['Case']['fields']['name']['type']='enum';
$dictionary['Case']['fields']['name']['options']='cases_names';
?>
```
```

Save the file and name it `vardefs.ext.php`. Notice that the file is quite small, but rather powerful. The first line overrides the default type defined for the name field and changes it to enum, which is the value that SugarCRM uses for drop-down fields. The second line specifies the name of the drop-down list that the field should use. As mentioned earlier, the drop-down list containing the values we want to use is named cases_names.

Next, you will need to place the file in the directory named `sugar install directory/custom/Extension/modules/Cases/Ext/Vardefs`. Proceed to create the directory structure manually if it does not already exist, making sure to match the casing as illustrated earlier.

Lastly, the changes need to be applied to the SugarCRM system. This is accomplished by using the **Repair** option within the admin control panel. Login to SugarCRM as an admin level user, and then select **Admin | Repair | Quick Rebuild & Repair | Repair** to apply the modifications. _If you do not perform this step, your modifications will not appear!_

Once the repair process finishes, check your cases module and it should now reflect the name field as a drop-down field.

Let us extend this example a bit further and assume that we also want to highlight the **Assigned To** field by coloring its text in red when the detail view of a case is accessed. Doing so would help guide the user's eye to a critical piece of information and can be used for any other field you deem equally important.