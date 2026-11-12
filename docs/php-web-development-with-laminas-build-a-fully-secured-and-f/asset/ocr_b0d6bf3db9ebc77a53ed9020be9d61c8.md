# Summary

In this chapter, we started by discussing some agile development concepts, which are very important for software maintenance. After these concepts, we learned how to easily detect duplicated code in PHP programs using the free and open source phpcpd tool. After detecting blocks of duplicated code to filter and validate in model classes, we showed how to unify these blocks in an extension of the Laminas\InputFilter\InputFilter class.

Next, we learned how to create a product search box. For this, we had to modify the ProductTable class to execute SQL SELECT queries with the LIKE operator.

Then, we learned how to generate API documentation using the free and open source swagger-docs tool. When we demonstrated how to use the OpenAPI Specification to generate rich HTML pages with REST endpoints, we also learned how to implement REST requests to list collections.

Finally, we learned how to isolate identity management to avoid security issues when we have more than one authentication control.

In the next chapter, we will introduce some tips and tricks that help devselopers to solve some problems that can eventually appear in the construction of an object-oriented PHP web application.