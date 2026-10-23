> **Important note**  
> Before you start writing your custom plugin, please make sure it does not already exist in the Gatsby Plugin Library.

There are multiple benefits to using a plugin from the Gatsby Plugin Library. Some of them are listed here:

+   - **Time-saving:** If a plugin fully accomplishes your task, you can install it, configure it, and start using it. If a plugin partially accomplishes your task, you can extend it by adding your custom functionality on top of it.
+   - **Bug fixing:** You or other developers may fix bugs and publish the information on npm, and get these fixes by updating the plugin in your application.
+   - **Improvement:** Same as bug fixing—you or other developers may add new functionality to the plugin and publish it on npm.
+   - **Security:** You or other developers may find security issues with the code or dependencies in a plugin; update the plugin with the latest security release and publish it on npm.

To find a Gatsby plugin, visit the Gatsby Plugin Library at `https://www.gatsbyjs.org/plugins/`.

The following page provides a **Search Gatsby Library** textbox that lets you navigate through more than 2,000 plugins! The textbox is on the top left-hand side of the page, as illustrated in the following screenshot:

![](bbox=[0.116, 0.596, 0.854, 0.704])

*Figure 9.1 – The Gatsby Plugin Library page*

Each Gatsby plugin comes with detailed instructions on how to install, configure, and use it. The Gatsby Source plugins allow Gatsby to get the content from a source such as a database, file, RESTful API (where **REST** stands for **Representational State Transfer**), and so on.

The `Starter` project that we installed in *Chapter 2, Introduction to Sanity* comes with the `gatsby-source-sanity` plugin already installed and configured. Throughout the `gatsby-source-sanity` plugin, we are able to get content from our Sanity application.