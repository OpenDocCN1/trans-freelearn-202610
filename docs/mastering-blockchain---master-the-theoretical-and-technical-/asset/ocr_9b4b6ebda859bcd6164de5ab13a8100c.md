# Compilation using truffle

Both libraries and contracts can be compiled using truffle. It is expected that the name of the contract file will be the same as the contract name within the file. For example, from the sample MetaCoin project created earlier, the file named `MetaCoin.sol` under the `contracts` directory has the same name as the MetaCoin contract in the file. This applies to library files too and it is case-sensitive.

+   - Filename:
+   
+     `MetaCoin.sol`
+   - Contract name within the file:
+   
+     ```
+     contract MetaCoin {
+         mapping (address => uint) balances;
+     ```

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

# Migration

This is the process by which truffle deploys contracts to the blockchain. This process relies on the files available under the `migrations` directory.