Once the console is available, various methods can be run in order to query the contract. A list of methods can be displayed by typing the following command and tab-completing:

```
dreguinox@dreguinox-OP7010:~/testdapp$ truffle console
truffle(default)> MetaCoin.
MetaCoin.__defineGetter__      MetaCoin.__defineSetter__      MetaCoin.__lookupGetter__      MetaCoin.__lookupSetter__
MetaCoin.__proto__             MetaCoin.constructor           MetaCoin.hasOwnProperty        MetaCoin.isPrototypeOf
MetaCoin.propertyIsEnumerable  MetaCoin.toLocaleString        MetaCoin.toString             MetaCoin.valueOf

MetaCoin.apply                 MetaCoin.arguments             MetaCoin.bind                 MetaCoin.call
MetaCoin.caller                MetaCoin.length                MetaCoin.name

MetaCoin.abi                   MetaCoin.address               MetaCoin.all_networks         MetaCoin.at
MetaCoin.binary                MetaCoin.checkNetwork          MetaCoin.class_defaults       MetaCoin.contract_name
MetaCoin.currentProvider       MetaCoin.defaults              MetaCoin.deployed             MetaCoin.events
MetaCoin.extend                MetaCoin.generated_with        MetaCoin.link                 MetaCoin.links
MetaCoin.network_id            MetaCoin.networks              MetaCoin.new                  MetaCoin.next_gen
MetaCoin.prototype             MetaCoin.setNetwork            MetaCoin.setProvider          MetaCoin.unlinked_binary
MetaCoin.updated_at            MetaCoin.web3
```

Exposed methods

Other methods can also be called in order to interact with the contract; for example, in order to retrieve the address of the contract, the following method can be called in the truffle console:

```
truffle(default)> MetaCoin.deployed().address
'0x0be9c5de978fa927b93a5c4faab31312cea5704a'
truffle(default)>
```

+   - Query the balance of the contract:

```
truffle(default)>
MetaCoin.deployed().getBalance.call(web3.eth.accounts[0])
{ [String: '8750'] s: 1, e: 3, c: [ 8750 ] }
```

The output returns a string with the value 8750.

+   - Transfer the balance:

```
truffle(default)>
MetaCoin.deployed().sendCoin("0xcce6450413ac80f9ee8bd97ca02b92c065d77abc",50, {from:"0x76f11b383dbc3becf8c5d9309219878caae265c3"})
'0xb8969149fcfb54ec9beac31af1fc86c386f9aa42cb13d2eb9bf9469931986e0f'
```

This will return the hash of the transaction and, when successful, it will result in increasing the balance of the target by 50. The target account is the argument passed in the sendCoin function.