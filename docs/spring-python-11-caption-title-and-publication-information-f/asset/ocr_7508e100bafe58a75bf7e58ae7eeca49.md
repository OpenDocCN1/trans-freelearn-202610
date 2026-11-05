Even though these are the default settings, it is easy to support another schema. Just override the variables as shown below.

```
@Object
def user_details_service(self):
    service = DatabaseUserDetailsService()
    service.dataSource = self.factory()
    service.DEF_USERS_BY_USERNAME_QUERY = "<alt user qry>"
    service.DEF_AUTHORITIES_BY_USERNAME_QUERY = "<alt auth qry>"
    return service
```

# Configuring LDAP-based security

LDAP is a popular protocol used to host user accounts and group associations as well. Spring Python provides a convenient LdapAuthenticationProvider with flexible options to support conventional schemas as well as custom ones.

> At the time of writing, Spring Python LDAP only works on CPython and not on Jython.

LdapAuthenticationProvider has two components:

+   - A BindAuthenticator and a PasswordComparisonAuthenticator to perform authentication
+   - A DefaultLdapAuthoritiesPopulator to lookup granted authorities based on conventional LDAP group associations

LDAP supports binding, where the server confirms the user's password. BindAuthenticator utilizes this feature to confirm the user's password.

It is also possible to fetch the password from the directory server, and do a password comparison after properly hashing the user's password. PasswordComparisonAuthenticator is used to perform this operation.

After confirming a user's credentials, the second step of LdapAuthenticationProvider is to search the directory for groups the user is associated with.