# First database creation is authorized

If phpMyAdmin detects that we have the right to create a database, the homepage appears as shown in the following figure:

![](bbox=[0.31, 0.21, 0.69, 0.29])

In the input field, a suggested database name appears if the $cfg['SuggestDBName'] parameter is set to TRUE – which is the default setting. The suggested database name is built according to the privileges we possess.

If we are restricted to the use of a prefix, the prefix might be suggested in the input field. (A popular choice for this prefix is the username.) Note that, in this case, the prefix is followed by an ellipsis mark. We should remove this ellipsis mark and complete the input field with an appropriate name.

![](bbox=[0.3, 0.45, 0.7, 0.53])

The Collation choice can be left unchanged for now; but with this dialog, we could pick a default character set and collation for this database. This setting can be changed later (see Chapter 10 for more explanation on this).

We will assume here that we have the right to create a database named marc_book. We enter marc_book in the input field and click on Create. Once the database is created, we will see the following screen:

![](bbox=[0.18, 0.67, 0.82, 0.82])