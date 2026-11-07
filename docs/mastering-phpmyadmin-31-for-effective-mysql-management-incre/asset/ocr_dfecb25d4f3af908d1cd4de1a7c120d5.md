Even considering those limitations, this mode may be a valuable choice for the following reasons:

+   - Some browsers can store the authentication information in an unencrypted form
+   - It is a bit faster than cookie processing

# Cookie authentication

The cookie authentication mode is superior to http in terms of the functionalities offered. It offers true login and logout, and can be used with PHP running on any kind of web server. It presents a login panel (see the following figure) from within phpMyAdmin. This can be customized, as we have the application source code. However, as you may have guessed, for cookie authentication, the browser must accept cookies coming from the web server — true for all authentication modes starting with phpMyAdmin 2.8.0.

This mode stores the username typed in the login screen into a permanent cookie in our browser. The password is stored as a temporary cookie. In a multi-server configuration, the username and password corresponding to each server are stored separately. To protect the username and password secrecy against some attack methods that target cookie content, they are encrypted using the Blowfish mechanism. So, to use this mode, we have to define (once) in config.inc.php, a secret password that will be used to securely encrypt all passwords stored as cookies from this phpMyAdmin installation.

This is done by putting a secret password here:

```
$cfg['blowfish_secret'] = 'SantaLivesInCanada';
```

Then, for each server-specific section, use the following:

```
$cfg['Servers'][$i]['auth_type'] = 'cookie';
```