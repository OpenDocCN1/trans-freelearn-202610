| XSQL Tag | Description | Attributes |
|---|---|---|
| xsq:set-cookie | Sets the value of an HTTP cookie. | name (required):
Name of HTTP cookie.
domain: Domain in which cookie is valid and readable. Default value is complete domain of document creating the cookie.
ignore-empty-value: Specifies if empty value is to be ignored. Default value is 'no'.
max-age: Maximum age of cookie in seconds. Default age is the user current browser session.
only-if-unset: Specifies if cookies should be set only if cookie is currently not defined. Default value is 'no'.
path: Relative URL within the domain in which cookie is valid and readable. Default value is the URL path of the document creating the cookie.
value: Cookie value. |

# Setting the Environment

In this section, we will discuss the preliminary setup required to create an XSQL application, which is not much, as the XSQLServlet is specified in web.xml and the XSQL libraries are added to the project when an XSQL page is created. Oracle database 11g JDBC drivers may be used with any Oracle database version 9i and later. We will use Oracle database 10g. We need to install Oracle database 10g including sample schemas. We need to create and start an instance of the Oracle Database. Connect to the database with the Order Entry (OE) sample schema (username).

```
CONNECT OE/<password>
```