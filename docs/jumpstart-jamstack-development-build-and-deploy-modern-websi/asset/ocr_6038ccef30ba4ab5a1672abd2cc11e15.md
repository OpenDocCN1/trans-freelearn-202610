The main topics that we will cover in this chapter are the following:

+   - Introduction to Netlify
+   - Netlify plugins
+   - Advanced configuration through the netlify.toml file
+   - Netlify deployment via the command line
+   - Azure Static Web Apps deployment

# Technical requirements

To deploy the project on Netlify, you must have an account at https://www.netlify.com/. To deploy the project on Azure, you must create an account at https://azure.microsoft.com/en-us/.

Additionally, a Terminal program is needed.

# Introduction to Netlify

Netlify, to someone who is new to web development, initially looks like a deployment and hosting platform.

In fact, a Netlify website can be a simple static file web host. Using Netlify's drag and drop feature, a folder of web files can be deployed as a real, functional, and hosted website in seconds, as shown:

![](bbox=[0.28, 0.61, 0.7, 0.69])

Figure 14.1 – Netlify's drag and drop deployment interface

However, while it is a place to host your website, it is so much more than that.

Firstly, behind the scenes, when we initially created our Sanity and Gatsby-generated website, the code provided by Sanity and Gatsby Blog Template automatically runs it, thereby creating an integration between Sanity and Netlify.