The required extension is contained in a file that can be named mysql.so or mysqli.so on Linux or UNIX, or mysql.dll (maybe mysqli.dll) on Windows. If our PHP server comes from a software package, we can find and install another software package—probably called php-mysql or php-mysqli. (The name is distribution dependent.) Otherwise, we can compile our own PHP server with the appropriate extension, as explained in the PHP documentation.

# #2003 - Can't connect to MySQL server

This message indicates that the MySQL server is not running, or cannot be reached from the web server. It can also be a socket (Linux/UNIX) or a named pipe (Windows) configuration problem.

# Socket problem (Linux/UNIX)

The socket configured in php.ini (an example of which is given below) does not correspond to the socket of the running MySQL server.

```
mysql.default_socket = /tmp/mysql.sock
```

As a result, PHP cannot reach MySQL. We can change it to:

```
mysql.default_socket = /var/lib/mysql/mysql.sock
```

However, to be sure, we must find the exact location of this socket.

# Named pipe problem (Windows)

This is a problem similar to that indicated above, but on Windows. It can be solved by adjusting mysql.default_socket with the correct named pipe used to connect locally to a MySQL server. For example:

```
mysql.default_socket = MySQL
```

# MySQL said: Access denied

This error can be solved when we understand the relevant login parameters.

# When using http authentication

We cannot use the web server security mechanism based on a .htaccess file and the http authentication in config.inc.php together. As a workaround, use cookie as the authentication type instead of http.