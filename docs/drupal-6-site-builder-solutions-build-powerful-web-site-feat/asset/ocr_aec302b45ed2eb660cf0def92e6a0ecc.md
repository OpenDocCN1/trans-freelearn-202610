Chef Wanyama likes to rotate his menu seasonally to ensure that he is always including the freshest ingredients in his dishes. To support this, we will allow him to specify whether each item is seasonal or not, and if it is seasonal what dates the menu should be available on. The display of the menu will take these dates into account, to display only those menu items that are currently available.

# Basic steps

+   1. We will begin by adding the Seasonal field, which is a text field, using the Select list widget. This field is added just like the Price field, except that we will use Select list for the Widget type. We will ensure that the valid values are only Yes and No by setting the Allowed values list, as shown in the following screenshot:
+   2. To add the start and end dates, you will need to add a second field, using a Date field.
+   3. In order to access the Date field, you must first download the Date Module from the Drupal site and install it on your site. As a minimum, you will need to make sure that you activate the Date API, Date, Date Timezone, Date Popup, and Calendar modules.

![](bbox=[0.1, 0.32, 0.95, 0.56])