You can search for a plugin on npm from the search tool on the following website:

```
https://www.npmjs.com
```

On the **package** page on the aforementioned website, you can find the description of a package, with installation instructions, dependencies, and versions. In the next section, we will explore the package versioning method that uses semantic versioning.

# Semantic versioning

In software development, developers assign a unique version number to a unique state of the software. Versioning a software helps developers to choose the right version needed to implement their software. This applies to packages, too.

When a package is created and added to npm, the first recommended version is 1.0.0. With further implementation and bug fixing, the version number increments depending on the type of change introduced.

The packages must follow major.minor.patch semantic versioning, detailed as follows:

+   - The *major* version increments by one when a developer introduces incompatible API changes. The version changes from 1.0.0 to 2.0.0.

+   - The *minor* version increments by one when a developer introduces a backward-compatible new functionality. The version changes from 1.0.0 to 1.1.0.

+   - The *patch* version increments by one when a developer introduces a backward-compatible bug fix. The version changes from 1.0.0 to 1.0.1.

Next, we will see how to install a package from npm.

# Installing a package from npm

To install a package from npm, from your Terminal, run the following command:

```
npm i <package_name>
```

Here, <package_name> is the name of the package. For example, if you would like to install a package from npm called birdie-hop, you can run the following command on your Terminal:

```
npm i birdie-hop
```

With the preceding command, npm will get the latest stable release of the birdie-hop package into your application.