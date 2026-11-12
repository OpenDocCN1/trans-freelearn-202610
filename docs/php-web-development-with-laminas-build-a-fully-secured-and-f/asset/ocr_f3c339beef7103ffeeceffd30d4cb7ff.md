+   2. Make sure the **Project name** field is filled with `school12`; then, click on **Copy**.

+   3. Select the `school11` project with the right mouse button and click on the **Close Project** item.

We are closing our first project because we don't need it anymore and if it stays open, we may accidentally change a file in that project thinking it belongs to the `school12` project. From now on, we will work on the `school12` project.

# # Use case – managing students

In this second project (`school12`), we will implement one more use case, *managing students*. This use case is very similar to *managing school classes*. Both have four flows, related to create, recover, update, and delete actions. The difference is that the `student` model is dependent on the `school class` model. So, a precondition for the flows of the *managing students* use case is that the `classes` table must have at least one record.

# # Implementing the use case

By now, you know how to create folders. So, let us implement a use case by following these steps:

+   1. Create a new folder named `view` in the root of the project.

+   2. Inside this folder, create two more folders: `schoolclass` and `student`.

The `view` folder is an explicit division for files related to the user interface. This organization will help us to identify what belongs to the presentation layer.

+   3. Next, select the following files:

+   • `delete.php`
+   • `edit.php`
+   • `index.php`
+   • `list.php`
+   • `save.php`

+   4. Copy these files to the `view/schoolclass` and `view/student` folders.

+   5. Now, you can remove all of them from the project root, except for the `index.php` file. The `index.php` file is the starting point of the application and will act as a coordinator for the other files. The `index.php` file is akin to the front door of a house. You need to get into the house to access the rooms. In a similar way, a user will have to use `index.php` before they access the other PHP files.