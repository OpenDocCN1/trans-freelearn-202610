```
user-search-base="ou-users"
user-search-filter="uid={0}"
group-role-attribute="businessCategory" />
</authentication-provider>
</authentication-manager>
```

The <ldap-user-service> configures LdapUserDetailsService (an implementation of UserDetailsService that we discussed earlier), which loads user details containing username, password, and roles from the LDAP server. Authentication is performed by comparing the user entered password with the user details loaded by the LdapUserDetailsService instance.

The <ldap-user-service> element accepts the following attributes:

+   user-search-base: Specifies the part of the directory tree under which search for users is performed. The value ou-users means that the search will be performed on entries that are defined under DN, which are ou=users, dc=sample, dc=com.

+   user-search-filter: It specifies the filter criteria used for searching users in the directory tree. The value uid={0} means that search is made for the user entry where value of uid attribute is equal to username entered by the user in the login form. The value {0} is replaced by the username entered by the user in the login form.

+   group-search-base: It specifies the part of the directory tree under which search for groups is performed. The value ou-groups means that the search will be performed on entries that are defined under DN, which are ou=groups,dc=sample, and dc=com.

+   group-search-filter: It specifies the filter criteria used for searching groups in the directory tree. The value uniqueMember={0} means that a search is made for group entries, where the value of the uniqueMember attribute is equal to the DN of the user.

If user ashish attempts to log in to the Flight Booking application, then the value of the user-search-base and user-search-filter attributes will be used for searching the user. This will result in returning the entry whose DN is uid=ashish,ou=users,dc=sample,dc=com. Now, the value of the group-search-filter and group-search-base attributes will be used to search for the group entry whose uniqueMember attribute value is uid=ashish,ou=users,dc=sample,dc=com. This will return the cn=appuser,ou=groups,dc=sample,dc=com entry because it contains the uniqueMember attribute with value uid=ashish,ou=users,dc=sample,dc=com.

+   group-role-attribute: It specifies the attribute of the group entry, which is used as the role name. As the value of group-role-attribute is businessCategory, if the group entry returned for the authenticating user is cn=administrator,ou=groups,dc=sample,dc=com, then the role of the user is admin_user—the value of the businessCategory attribute of the entry.