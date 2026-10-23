Here, <package_name> is the name of the package you want to update. If you want to update the `birdie-hop` package, you can run the following command:

```
```n
pm update birdie-hop
```
```

During updating or installation, npm will check the `package.json` file and download or update a plugin to the allowed version.

# # The package.lock file

When you are working in a team, you want all the developers in your team to use the same package versions. In this situation, the `package.lock` file comes in handy. It is automatically generated when you modify the `package.json` file or the `node_modules` folder. It lists all the exact versions of the packages the application should use. The `package.lock` file will avoid a situation where two developers in the same team working on the same project use two different versions of the `birdie-hop` package.

In this section, we have explored the main components of a package. We learned where to find the packages and how to install them. We learned about package versioning and how to install and update a specific package to a specific version. We have also explored how `package.json` and `package.lock` files play an important role in making sure your application gets the right package with the right version.

In the next section, we will install a package in our Gatsby application.

# # Searching, installing, and configuring plugins

In this section, we will explore the Gatsby public plugin repository. We will look at how to install a Gatsby plugin and where to find the configuration for the plugins you would like to install.

We will install and configure the `gatsby-source-filesystem` and `gatsby-source-drupal` plugins and see how we can use them.

# # The Gatsby Plugin Library

Gatsby also provides a plugin library, a collection of npm packages tagged with the *Gatsby* keyword. In this library, it is possible to find standard plugins that will be used on most sites, related to **Search Engine Optimization (SEO)**, **Really Simple Syndication (RSS)**, social media buttons, third-party comment services, e-commerce integration, and more.