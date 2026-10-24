
Once the command is successful, it will create the directory structure shown here. This can be viewed using the `tree` command in Linux:

```text
drequinox@drequinox-OP7010:~/testdapp$ tree
.
├── app
│   ├── images
│   ├── index.html
│   ├── javascripts
│   │   └── app.js
│   └── stylesheets
│       └── app.css
├── contracts
│   ├── ConvertLib.sol
│   ├── MetaCoin.sol
│   └── Migrations.sol
├── migrations
│   ├── 1_initial_migration.js
│   └── 2_deploy_contracts.js
├── test
│   └── metacoin.js
└── truffle.js
7 directories, 10 files
```

This command creates four main directories, named `app`, `contracts`, `migrations`, and `test`. As seen in the preceding example, a total of 7 directories and 10 files have been created. In the following section, an explanation of all these files and directories will be presented.

- **App:** This directory contains all application files including HTML files, images, style sheets and JavaScript files. This folder contains further subdirectories, images, javascripts, and stylesheets that contain relevant application files.
- **Contracts:** This directory contains solidity contract source code files. This is where truffle will look for solidity contract files during migration.
- **Migration:** This directory has all the deployment scripts.
- **Test:** As the name suggests, this directory contains relevant test files for applications and contracts.

Finally, truffle configuration is stored in the `truffle.js` file, which is created in the root folder of the project from where `truffle init` was run. When `truffle init` is run, it will create a sample project named MetaCoin. As an example, you will first be introduced to how to use various commands in truffle in order to test and deploy MetaCoin. Later, further examples will be shown on how to use truffle for custom projects.

## Compilation using truffle

Both libraries and contracts can be compiled using truffle. It is expected that the name of the contract file will be the same as the contract name within the file. For example, from the sample MetaCoin project created earlier, the file named `MetaCoin.sol` under the `contracts` directory has the same name as the MetaCoin contract in the file. This applies to library files too and it is case-sensitive.

- Filename:

  `MetaCoin.sol`

- Contract name within the file:

  ```
  contract MetaCoin {
      mapping (address => uint) balances;
  }
  ```

Compilation can be run as shown here:

```
~/testdapp$ truffle compile
Compiling ConvertLib.sol...
Compiling MetaCoin.sol...
Compiling Migrations.sol...
Writing artifacts to ./build/contracts
~/testdapp$
```

Once the compilation is finished successfully, all objects will be written in to the `build` directory. The output directory looks like what is shown here:

```
~/testdapp$ tree build/
build/
└── contracts
    ├── ConvertLib.sol.js
    ├── MetaCoin.sol.js
    └── Migrations.sol.js
1 directory, 3 files
```

As shown in the preceding example, the `build` directory is created automatically with the `contracts` subdirectory, which contains three JavaScript files.

## Migration

This is the process by which truffle deploys contracts to the blockchain. This process relies on the files available under the `migrations` directory. The process works as shown here:

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
drequinox@drequinox-OP7010:~/testdapp/migrations$ cat 1_initial_migration.js
module.exports = function(deployer)
{
  deployer.deploy(Migrations);
};
```

As shown in the preceding output, there are two files that contain the code that specifies which contracts are required to be deployed.

The filenames follow a convention where they are required to be prefixed by a number. This prefix is required in order to keep a record of all migrations. The suffix in the filename can be any descriptive name. First, it is important to change the `truffle.js` file in order to point to the appropriate network. The `truffle.js` file contains valuable information about the build and RPC for the application. In this case, `geth` is already running and it can simply be pointed to use the available client:

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

First, list the accounts to see all accounts and then select the account that needs to be unlocked. Truffle assumes the coinbase account by default. Select the appropriate account, as follows:

```
> personal.listAccounts
["0x76f11b383dbc3becf8c5d9309219878caae265c3",
"0xcce6450413ac80f9ee8bd97ca02b92c065d77abc"]
```

The account can be unlocked using the following command:

```
> personal.unlockAccount("0x76f11b383dbc3becf8c5d9309219878caae265c3")
Unlock account  0x76f11b383dbc3becf8c5d9309219878caae265c3
Passphrase:
true
```

Once the account is unlocked, migration can be run again using the following command:

```
~/testdapp$ truffle migrate
```

It will show output similar to that shown here. It should be noted that mining must be started for the migration to finish. Migration will perform various steps by finding the files available in the migrations directory. As shown in the example, `1_initial_migration.js` and `2_deploy_contracts.js` have been used to provide migration steps and requirements to truffle:

```
Running migration: 1_initial_migration.js
  Deploying Migrations...
  Migrations: 0xf444cce0cee00cab4d04bcfc0005626b8b02add8
  Saving successful migration to network...
  Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying ConvertLib...
  ConvertLib: 0x2ba8a4a75a6b845bf482923cff29ecc98cd68d90
  Linking ConvertLib to MetaCoin
  Deploying MetaCoin...
  MetaCoin: 0x0be9c5de978fa927b93a5c4faab31312cea5704a
  Saving successful migration to network...
  Saving artifacts...
~/testdapp$
```

Once the command is completed successfully, it will return a command prompt displaying the message saving artefacts.

Deployment can be verified using a few commands shown here via the geth JavaScript console:

```
> eth.getBalance("0x0be9c5de978fa927b93a5c4faab31312cea5704a")
0
> eth.getCode("0x0be9c5de978fa927b93a5c4faab31312cea5704a")
"0x606060405260e060020a60003504637bd703e8811461003457806390b98a1114610056578063f8b2cb4f1461007d575b610000565b346100005761004460043561009f565b60408051918252519081900360200190f35b34610000576100696004356024356101195b565b604080519115158252519081900360200190f35b346100005761006100446004356101b1565b60408051918252519081900360200190f35b6000732ba8a4a75a6b845bf482923cff29ecc98cd68d906396e4ee3d6100c4846101b1565b60026000604051602001526040518360e060020a028152600401808381526020018281526020019250505060206040518083038186803b156100005760325a03f415610000575050604051519150505b919050565b600160a060020a033316600090815260208190526090205482901015616101425750560000610a1b565b600160a060020a0600020a03381260008181526020818152604080832080548890039055938716808352918490208054870190558351868152935191937fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef929081900390910190a35060015b92915050565b600160a060020a0381166000908152602081905260409020545b91905056"
```

Note that the address of the newly deployed contract has been taken from the truffle migrate command output shown earlier. (MetaCoin: 0x0be9c5de978fa927b93a5c4faab31312cea5704a)

- **Interaction with the contract:** Truffle also provides a console (command-line interface) that allows interaction with the contracts. All deployed contracts are already instantiated and ready to use in the console. This is an REPL-based interface that means Read, Evaluate, and Print Loop. Similarly, in the geth client (via attach or console), REPL is used via exposing **JSRE (JavaScript runtime environment)**. The console can be accessed by issuing the following command:

  ```
  ~/testdapp$ truffle console
  ```

This will open a command-line interface, as shown here:

![](img/62ab880d2302f959d34db648cb0bec5e_356_0.png)

Truffle console
