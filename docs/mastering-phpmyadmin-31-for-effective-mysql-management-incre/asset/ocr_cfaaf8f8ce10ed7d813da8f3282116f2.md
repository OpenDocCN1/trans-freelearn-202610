Here, **reload the privileges** is clickable. The effective privileges (the ones against which the server bases its access decisions) are the privileges that are located in the server's memory. Privilege modifications, to be made from the **User overview** page, are made both in memory and on disk, in the mysql database. Modifications made directly to the mysql database do not have immediate effect. The **reload the privileges** operation reads the privileges from the database and makes them effective in memory.

# Adding a user

The **Add a new User** link brings a dialog for user account creation. First, we see the panel where we'll describe the account itself.

![](bbox=[0.18, 0.35, 0.82, 0.545])

The second part of the **Add a new User** dialog is where we'll specify this user's global privileges, which apply to the server as a whole (see the *Global privileges* section).

![](bbox=[0.22, 0.63, 0.78, 0.83])