This assigns privileges only for the chosen database. We can also choose **Use text field** and enter a database name. We could insert a non-existent database name, so that this user can create it later (provided we give him/her the CREATE privilege in the next panel). We can also use special characters, such as the underscore and the percent sign for wildcards.

For example, entering **bill** here would enable him to create a **bill** database, and entering **bill%** would enable him to create a database with any name that starts with **bill**. For our example, we will enter **bill**.

The next screen is used to set **bill**'s privileges on the **bill** database and create table-specific privileges.

To learn more about the meaning of a specific privilege, we can move the mouse over a privilege name (which is always in English), and an explanation about this privilege appears in the current language. We give **SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER, INDEX, and DROP** privileges to **bill** on this database. We then click **Go**.

![](bbox=[0.185, 0.425, 0.815, 0.785])