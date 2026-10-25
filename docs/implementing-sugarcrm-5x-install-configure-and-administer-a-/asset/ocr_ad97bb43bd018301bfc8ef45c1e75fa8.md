# # Enhanced search

All of the tools that we have examined so far have one thing in common; they
provide new functionality to SugarCRM. However, none of the previous tools
modify the behavior of existing features. The Enhanced Search functionality
will be our first trek into this territory.

As stated earlier in this book, the available search features in SugarCRM provide a
very helpful way of locating data within the system, but at the same time are rather
limited in their capabilities. A common problem that users encounter is that search
behavior cannot be changed from the default AND connector to OR. The inability
to do so presents a number of challenges that are magnified if your search criterion
requires the searching of more than one field.

For example, if you wish to search for contacts whose office numbers begin with
the digits 310 or their State field is set to California, you would find that the default
search feature will only return records that have an office number that begin with
310 and are also in California; this is not the set of records you are looking for.
Enhanced Search modifies this existing functionality to remove this limitation
and in general, provide a richer search experience in many different ways.

Installation is performed through the Module Loader and when the installation
completes its functionality, it immediately becomes available to all SugarCRM
users. Once installed, the Advanced Search panel on your list view screens
should resemble the following:

![](bbox=[0.211, 0.539, 0.745, 0.895])