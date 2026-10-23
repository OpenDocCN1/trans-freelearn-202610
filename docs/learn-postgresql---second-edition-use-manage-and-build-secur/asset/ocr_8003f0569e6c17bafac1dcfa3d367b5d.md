You will learn more about RLS in the RLS section of this chapter.

# Changing properties of existing roles: the ALTER ROLE statement

As you can imagine, once they have been created, roles are not immutable: you can add or remove properties to or from a role by means of the ALTER ROLE statement. The synopsis for the statement is very similar to the one used to create a role, and is as follows:

```
ALTER ROLE name [ [ WITH ] option [ ... ] ]
```

Here, name is the unique role name and the options are specified in the exact same manner as in the CREATE ROLE statement.

As an example, imagine you want to provide the luca role with the capabilities to create databases and new roles. You can issue two ALTER ROLE statements or combine the options as follows:

```
forumdb=# ALTER ROLE luca WITH CREATEDB;
ALTER ROLE

forumdb=# ALTER ROLE luca WITH CREATEROLE;
ALTER ROLE

-- same as the above two statements
forumdb=# ALTER ROLE luca CREATEROLE CREATEDB;
ALTER ROLE
```

And if you, later on, change your mind, you can remove one or both options by assigning the negated form:

```
forumdb=# ALTER ROLE luca NOCREATEROLE NOCREATEDB;
ALTER ROLE
```

The ALTER ROLE statement is always executable by a cluster superuser, but can also be executed by a non-superuser role that has the CREATEROLE option (that is, can create, and therefore manipulate, other roles), as long as the statement is applied to a non-superuser role.

# Renaming an existing role

The ALTER ROLE statement also allows for a change in the name of the role: the RENAME clause allows for a role to be substituted by another unique role name. As an example, let’s change a role’s short username to a longer one: