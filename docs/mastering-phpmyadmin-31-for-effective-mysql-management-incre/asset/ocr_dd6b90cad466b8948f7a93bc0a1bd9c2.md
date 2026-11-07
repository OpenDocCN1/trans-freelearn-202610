![](bbox=[0.205, 0.19, 0.255, 0.225])

> It might be tempting to open the book table on its Structure page and look for a list of procedures that manipulate this table, such as our add_page() procedure. However, all procedures are stored at the database level, and there is no direct link between the code itself (UPDATE book) and the place where the procedure is stored.

# Manually creating a function

Functions are similar to stored procedures. However, a function may return just one value, whereas a stored procedure can have more than one OUT parameter. On the other hand, using a stored function from within a SELECT statement may seem more natural because it avoids the need for an intermediate SQL variable to hold the value of an OUT parameter.

What is the goal of functions? As an example, a function can be used to calculate the total cost of an order, including tax and shipping. Putting this logic inside the database, instead of at the application level, helps document the application-database interface. It also avoids duplicating business logic in every application that needs to deal with this logic.

We should not confuse MySQL 5.0 functions with UDF (User-Defined Functions), which existed prior to MySQL 5.0. A UDF constitutes code written in C or C++, compiled into a shared object, and referenced with a CREATE FUNCTION statement and the SONAME keyword.

phpMyAdmin's treatment of functions is in many ways similar to what we have covered in procedures:

+   - A query box to input a function
+   - The use of a delimiter
+   - A mechanism to manipulate a function that is already defined

Let's define a function that retrieves the country name, based on its code. I prefer to use a param_ prefix to clearly identify the parameters inside the function's definition, and a var_ prefix for local variables. We'll use our trusty SQL query box to input the function's code, again indicating to this box to use // as the delimiter.

```
CREATE FUNCTION get_country_name(param_country_code CHAR(2))
  RETURNS VARCHAR(50)
BEGIN
  DECLARE var_country_name VARCHAR(50) DEFAULT 'not found';
  SELECT description
```