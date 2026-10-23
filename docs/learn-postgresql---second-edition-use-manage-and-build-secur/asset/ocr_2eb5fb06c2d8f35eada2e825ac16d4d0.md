```
rolname | group | is_admin
----------+---------------+----------
enrico | book_authors | f
enrico | forum_admins | f
luca | forum_stats | f
luca | book_authors | f
pg_monitor | pg_read_all_settings | f
pg_monitor | pg_read_all_stats | f
pg_monitor | pg_stat_scan_tables | f
test | forum_stats | f
(8 rows)
```

# Roles that inherit from other roles

We already saw in *Chapter 3, Managing Users and Connections*, that a role can contain other roles, therefore behaving as a group.

When a role becomes a member of another role, it gets all the permissions of the containing (group) role. However, there are cases where such privileges are dynamically granted, that is, the member role will have the privileges transparently, and cases where the privileges will be granted statically, that is, the member role needs to explicitly become the group role in order to use its privileges. The INHERIT property of a role discriminates how roles can use privileges by default. The GRANT statement has an optional WITH INHERIT clause that can be specified when a role is added to a group: if the clause has a true value, then the added role will immediately and dynamically get all the permissions of the group, otherwise it will not.

If the WITH INHERIT option is not specified, then the role’s INHERIT property will be implicitly used. In order to understand the difference and the implication, let’s see how the forum_admins and forum_stats roles could have been built:

```
forumdb=# CREATE ROLE forum_admins WITH NOLOGIN;
CREATE ROLE

forumdb=# CREATE ROLE forum_stats WITH NOLOGIN;
CREATE ROLE

forumdb=# REVOKE ALL ON forum.users FROM forum_stats;
REVOKE
```