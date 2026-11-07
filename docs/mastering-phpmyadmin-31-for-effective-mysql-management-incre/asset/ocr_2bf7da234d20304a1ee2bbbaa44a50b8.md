# B

# Troubleshooting and Support

This appendix proposes guidelines for solving some common problems, and gives hints on how to avoid them. It also explains how to interact with the development team for support, bug reports, and contributions.

# System requirements

A section at the beginning of the Documentation.html file (which is included with the downloaded kit), discusses system requirements for the particular phpMyAdmin version we are using. It is crucial that these requirements be met, and that the environment be properly configured so that problems are avoided.

Some problems, such as phpMyAdmin bugs, are in fact caused by the server environment. Sometimes, the web server is not configured to interpret .php files correctly, or the PHP component inside the web server does not run with the mysqli or mysqli extensions. MySQL accounts may be badly configured. This can happen on home servers as well as on hosted servers.

When we suspect that something is wrong, we can try a simple PHP script, test.php, which contains the following to check if the PHP component answers correctly.

```
<?php
echo 'hello';
?>
```

We should see the hello message. If this works, we can try another script:

```
<?php
phpinfo();
?>
```