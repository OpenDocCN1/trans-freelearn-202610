To improve the speed of the queries that we will make by author ID, we should add an index on this field. The right part of our screen now looks like this:

![](bbox=[0.36, 0.19, 0.64, 0.37])

At this point, we could pick a different Storage Engine from the corresponding drop-down menu. However, for time being, we will just accept the default storage engine.

Now we are ready to create the table by clicking on Save. If all goes well, the next screen confirms that the table has been created; we are now in the Structure subpage of Table view.

![](bbox=[0.17, 0.51, 0.83, 0.72])

Should we forget to specify a value in the Length column for a CHAR or VARCHAR, phpMyAdmin would remind us before trying to create the table.