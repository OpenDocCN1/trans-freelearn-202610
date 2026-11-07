# User name

The User name menu offers two choices. We can choose Use text field and input a user name in the box, or we can choose Any user to create an anonymous user (the blank user). Let's choose Use text field and enter bill.

# Host

By default, this menu is set to Any host with % as the host value. The Local choice means "localhost". The Use host table choice (which creates a blank value in the host field) means to look in the mysql.hosts table for database-specific privileges. Choosing Use text field allows us to input the exact host value we want. Let's choose Local.

# Password

Even though it's possible to create a user without a password (with the No password choice), it's best to have a password. We have to enter it twice (as we cannot see what is entered) to confirm the intended password. Let's input bingo.

Some administrators prefer to improve security by having phpMyAdmin generate a password itself. In the Generate Password dialog, a click on Generate puts a random password (in clear) on screen. At this point, we should note the password so that we can pass it on to the user. Then a click on Copy puts this password in the Password and Re-Type fields.

# Database creation and rights

A frequent convention is to assign a user, the rights to a database, having the same name as this user. To accomplish this, the Database for user section offers the checkbox Create database with same name and grant all privileges,. Clicking this checkbox automates the process by creating both the database and the corresponding rights. Please note that, with this method, each user would be limited to one database (user bill, database bill).

Another possibility is to allow users to create databases that have the same prefix as their usernames. Therefore, the other choice Grant all privileges on wildcard name (username\_%) performs this function by assigning a wildcard privilege. With this in place, user bill could create the databases bill_test, bill_2, bill_payroll, and so on; phpMyAdmin does not pre-create the databases in this case.