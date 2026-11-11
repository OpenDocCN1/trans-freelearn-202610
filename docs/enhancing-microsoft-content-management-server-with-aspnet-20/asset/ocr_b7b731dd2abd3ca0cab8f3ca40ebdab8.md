# Importing the TropicalGreen Site Deployment Object File

Before we begin, let’s populate the database by importing objects using the Site Deployment Manager.

+   1. Download the `TropicalGreen.sdo` file from the book’s companion website.
+   2. Open **Site Manager** and log in with an MCMS administrator account.
+   3. From the menu, select **File | Package | Import...**.
+   4. In the Site Deployment Import dialog, click the **Browse...** button. Navigate to the `TropicalGreen_Final.sdo` file downloaded earlier.
+   5. In the **Container Rules** tab, set the following:
+   6. In the **Rights Group** tab, set the following:
+   7. Click **Import**.
+   8. The import confirmation dialog appears. Click **Continue**.

| Property | Value |
|---|---|
| When Adding Containers | Use package container rights |
| When Replacing Containers | Keep destination container rights |

| Property | Value |
|---|---|
| Select how Rights Groups are imported | Import User Rights Groups |

# Creating a New MCMS Web Application

To get started, let’s create a new MCMS web application using the project templates we created in the previous chapter.

+   1. From Visual Studio, from the **File Menu**, choose **New | Web Site**.
+   2. In the **New Web Site** dialog, select the **MCMS SP2 Web Application** icon in the **My Templates** section.
+   3. Select **HTTP** in the **Location** list box.
+   4. Enter **http://localhost/TropicalGreen** in the **Location** textbox, and click **OK**.