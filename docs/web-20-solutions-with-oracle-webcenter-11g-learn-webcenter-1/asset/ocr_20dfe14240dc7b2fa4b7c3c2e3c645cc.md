A **Blog** is a website that allows an individual author to post a stream of content for a community to view. The word "Blog" is derived from the contraction of "Web Log". Each item in the Blog stream can be viewed as an article. An article can be something as simple as a video or image posting, or something as formal as a newspaper article. It is up to the author to decide what content to post. Typically, authors allow members of a community to engage in dialog related to the blog article by allowing them to post comments on the article.

Both the Wiki Service and the Blog Service depend on the WebCenter Wiki and Blog Server. Both services use a single connection to the single backend server.

In this chapter, we will do the following:

+   - Walk you through creating a custom WebCenter application that leverages the Discussions Service
+   - Present a handful of task flows
+   - Show you where to find key pieces of information required to configure your Discussions Service connection in your custom application
+   - Learn how to integrate a custom WebCenter application with the Wiki and Blog Services, specifically, using an IFrame and using the Web Clipping Portlet

# # Discussions configuration

Our application is built with JDeveloper 11g and connects to the Oracle WebCenter Discussions Server. Ensure that the Discussions Server is running by connecting to the Discussions Server URL (http://hostname:8890/owc_discussions). It is a good idea to populate the Discussions Server with some example users, categories, and forums. The Forums Admin Console is used to create new users and forums. The admin console can be accessed at http://hostname:8890/owc_discussions/admin.

The default admin username is weblogic and the password is weblogic. This is irrespective of any username/password specified at installation time.

You can configure a wide variety of settings in the Admin Console, including but not limited to the following:

+   - Content Structure: Categories, Subcategories, Forums
+   - Users and Groups
+   - Permissions