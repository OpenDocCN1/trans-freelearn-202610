Adding the version with no character before the version will download that exact version if you have never installed the plugin in your app, or it will update the package to that very same version. Now, have a look at this code snippet:

```
npm i birdie-hop@~1.2.3
```

The tilde (~) tells npm that it can install or update the package to the latest patch release available. In this case, the version will be >1.2.3 and <1.3.0. Next, have a look at the following code snippet:

```
npm i birdie-hop@^1.2.3
```

The caret (^) tells npm that it can install or update the package to the latest minor release available. In this case, the version will be >1.2.3 and <2.0.0. Now, have a look at this code snippet:

```
npm i birdie-hop@*1.2.3
```

The asterisk (*) tells npm that it can install or update the package to any major version available. In this case, the version will be >1.2.3 and there is no limit on updates. Let's move on to understanding how to update a plugin.

By default, the packages are going under the dependencies array and will be required by your application in production. You can also set up packages that are only needed in your local development environment or on your testing environment, such as automated testing packages. This package will go under the devDependencies array in the packages.json file and can be added by adding the --save-dev flag, as follows:

```
npm i birdie-hop --save-dev
```

# Updating a plugin

When a plugin releases an update, we can update our plugin via npm. To update all packages listed in the package.json file, run the following command:

```
npm update
```

To update a single package, run the following command:

```
npm update <package_name>
```