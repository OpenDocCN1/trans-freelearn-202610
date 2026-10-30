# Getting ready

Access to a running MySQL server with administrator privileges is required to run the code shown in this recipe.

![](bbox=[0.136, 0.204, 0.214, 0.263])

> The previous recipe shows how to install the free Community Edition of MySQL Server. The administrator privileges will enable you to implement this recipe.

# How to do it…

First, we create a dictionary in the same module of the GUI_MySQL_class.py code:

```
```python
# create dictionary to hold connection info
dbConfig = {
    'user': <adminName>,       # use your admin name
    'password': <adminPwd>,    # not the real password
    'host': '127.0.0.1',       # IP address of localhost
}
```
```

Next, in the connection method, we unpack the dictionary values. Take a look at the following code snippet:

```
```python
mysql.connect('user': <adminName>, 'password': <adminPwd>, 'host':
'127.0.0.1')
```
```

Instead of using the preceding snippet, we use (**dbConfig), which achieves the same thing as the preceding one but is much shorter:

```
```python
import MySQLdb as mysql
# unpack dictionary credentials
conn = mysql.connect(**dbConfig)
print(conn)
```
```

This results in the same successful connection to the MySQL server, but the difference is that the connection method no longer exposes any mission-critical information: