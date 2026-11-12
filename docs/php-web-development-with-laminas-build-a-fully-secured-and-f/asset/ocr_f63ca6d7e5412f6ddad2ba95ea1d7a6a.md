# Creating the employee's registration

Maybe we offered a false promise in the last section. In fact, imagine you already have implemented a CRUD operation for employees, similar to what we did in Chapter 7, Request Control and Data View, for products. Also imagine that we have implemented this CRUD operation using laminas-form, laminas-filter, and laminas-validation, similar to what we did in Chapter 8, Creating Forms and Implementing Filters and Validators, for product discounts. It is not difficult to imagine because its source code is actually available in the repository mentioned in the Technical requirements section. As we are using patterns, the structure of CRUD implementations is the same. Therefore, we are assuming that there are the following classes in the module/Inventory/src/Controller folder:

+   - EmployeeAPIController
+   - EmployeeAPIControllerFactory
+   - EmployeeController
+   - EmployeeControllerFactory

Note that we have two pairs of classes – an API controller and its factory, and a web interface controller and its factory. It is exactly the same structure that we have used for product controller layer implementation. These controller classes are tested by the following classes in the module/Inventory/test/Controller folder:

+   - EmployeeAPIControllerTest
+   - EmployeeControllerTest

We also have a view layer implementation for discounts that is very similar to what we had made for products. Therefore, we are assuming that the following view scripts are in the module/Inventory/view/inventory/employee folder:

+   - edit.js
+   - edit.phtml
+   - index.js
+   - index.phtml

Here, we have two pair of files that generates two web pages – the inserting/updating page and the listing page. The .phtml files have a combination of HTML and PHP code, and the .js files have JavaScript code, exclusively.