This means that for the furniture catalog, a two-level structure is required, such as categories and products. Each product will belong to only one of the categories listed previously.

In the next step, you have to figure out what fields you need to create for one item (product) in your catalog. This is like making a table from your product's list. What columns do you need to separate different information? For my example, I need the following four fields:

+   - Product title
+   - Description
+   - Image
+   - Price

Now, you can start creating your own module. You are not going to use any special dedicated modules, but you will learn how to create an individual module perfectly fitted for your individual needs. This can be done with **CTLModuleMaker** module that allows the creation of any fully functional catalog-like module.

The example structure of the new product catalog is as follows:

+   - Desks
+     - Computer desk
+     - Executive desk
+   - Chairs
+     - Executive chair
+     - Guest chair
+   - Bookcases
+     - .....

Install the **CTLModuleMaker** module from the list of available modules (**Extensions | Module Manager**). When the module is installed, its admin area can be found in the admin console (**Extensions | CTLModuleMaker**).

# Creating a new catalog-like module

We will now start to create a new individual module. In the admin console, go to the admin area of **CTLModuleMaker** (**Extensions | CTLModuleMaker**), and click on the link **Create a new module**.