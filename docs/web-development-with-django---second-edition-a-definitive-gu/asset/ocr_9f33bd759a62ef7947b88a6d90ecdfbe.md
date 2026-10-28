> Note
> Before you proceed, you'll need to make sure the rest of the templates in the registration directory follow the bookr project's style; that is, they inherit from the Admin app's admin/base_site.html template. You've already seen this done with password_change_done.html and the login.html templates. Go ahead and apply what you've learned in this exercise (and the section before it) to the rest of the files in the registration directory. Alternatively, you may download the modified files from this book's GitHub repository: https://github.com/PacktPublishing/Web-Development-with-Django-Second-Edition/tree/main/Chapter09/Activity9.01/bookr.

Now that we have modified our project to enable authentication, we will learn about password and session storage in Django.

# Password storage in Django

Django does not store passwords in plain text form in the database. Instead, passwords are digested with a hashing algorithm, such as PBKDF2/SHA256, BCrypt/SHA256, or Argon2. As hashing algorithms are a one-way transformation, this prevents a user's password from being decrypted from the hash stored in the database. This often comes as a surprise to users who expect a system administrator to retrieve their forgotten password, but it is best practice in security design. So, if we query the database for the password, we will see something like this:

```
sqlite> select password from auth_user;
pbkdf2_sha256$320000$auohDtokS8a6LAoaG79Tv1$Mxzssu6gfd5uZvI1ADF I4faAjP0g3puP2JxxFdWZI0=
pbkdf2_sha256$320000$SqoVF8nhgo4zgw4juRUNtAK5TATQEqXtbRAfc4Ccuuwr9nRHVuShZo5W6N90ygpvVbA=
pbkdf2_sha256$320000$Ss3eprgniu7e4q0ux9Xj5nh$/SAFKgXuYIWSm08PWEwBUsMfmPCYd60rAaz0JZnLKGs=
```

Figure 9.4 – Password hashes in a Django SQLite3 database

The components of this string are <algorithm>$<iterations>$<salt>$<hash>. As several hashing algorithms have been compromised over time and we sometimes need to work with mandated security requirements, Django is flexible enough to accommodate new algorithms and can maintain data encrypted in multiple algorithms.

# The profile page and the request.user object

When a login is successful, the login view redirects to /accounts/profile. However, this path is not included in the existing auth.url, nor does the authentication app provide a template for it. To avoid a Page not Found error, a view and an appropriate URL pattern are required.