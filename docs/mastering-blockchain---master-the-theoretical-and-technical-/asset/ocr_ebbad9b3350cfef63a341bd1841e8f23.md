The process works as shown here:

```
```text
~/testdapp$ cd migrations/
~/testdapp/migrations$ ls -ltr
-rw-rw-r-- 1 drequinox drequinox 124 Dec 12 12:57 2_deploy_contracts.js
-rw-rw-r-- 1 drequinox drequinox  72 Dec 12 12:57 1_initial_migration.js
~/testdapp/migrations$ cat 2_deploy_contracts.js
module.exports = function(deployer)
{
  deployer.deploy(ConvertLib);
  deployer.autolink();
  deployer.deploy(MetaCoin);
};
drequinox@drequinox-OP7010:~/testdapp/migrations$ cat
1_initial_migration.js
module.exports = function(deployer)
{
  deployer.deploy(Migrations);
};
```
```

As shown in the preceding output, there are two files that contain the code that specifies which contracts are required to be deployed.

The filenames follow a convention where they are required to be prefixed by a number. This prefix is required in order to keep a record of all migrations. The suffix in the filename can be any descriptive name. First, it is important to change the `truffle.js` file in order to point to the appropriate network. The `truffle.js` file contains valuable information about the build and rpc for the application. In this case, `geth` is already running and it can simply be pointed to use the available client:

```
```javascript
module.exports = {
  build: {
    "index.html": "index.html",
    "app.js": [
      "javascripts/app.js"
    ],
    "app.css": [
      "stylesheets/app.css"
    ],
    "images/": "images/"
  },
  rpc: {
    host: "localhost",
    port: 8001
  }
};
```
```