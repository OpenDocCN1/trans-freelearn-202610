Now, at this point, the new account can be queried by using curl. Simply pass the address as an argument in the URL and the result will be returned in JSON format.

```
drequinox@drequinox-OP7010:~/testApp$ curl http://strato-dev4.blockapps.net/eth/v1.2/account?address=b16fe63de8fcf9d7f1d787cba37a02309104316
[{"contractRoot":"56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421","kind":"AddressStateRef","balance":"1000000000000000000","address":"b16fe63de8fcf9d7f1d787cba37a02309104316","latestBlockNum":79860,"code":"","nonce":0}]drequinox@drequinox-OP7010:~/testApp$
```

![](bbox=[0.105, 0.164, 0.895, 0.26])

Alternatively, a query can be executed via any web browser as shown in the following screenshot:

![](bbox=[0.105, 0.36, 0.895, 0.555])

In the next step, the process to upload the new contract to the test chain will be described. Note that all the contracts are placed in the `./app/contracts` directory under the `testApp` directory. As a sample, `Greeter.sol` contract has been chosen to be deployed to the network. BlockApps provides an easy method to achieve this deployment.

All contracts needs to be placed under the contracts directory in order for the compile command to find them and compile.