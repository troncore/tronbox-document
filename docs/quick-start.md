# Quick Started

To deploy a smart contract using a tronbox, you need to write a smart contract file, compile the smart contract, and finally migrate and deploy the smart contract. After that you can then use the script to test.

# Contract Development

All smart contract files need to be placed in the `./contracts` directory. By default, there will be a contract file with a suffix of `.sol`. But if you need to write a smart contract, you need to create a new .sol.

Tronbox requires the smart contract name to be the same as the file name. For example, if the file name is `./contracts/Test.sol`, we can write the contract as follows:
```js
pragma solidity >=0.4.23 <0.6.0;
contract Test{
    function f() public pure returns (string memory){
        return "method f()";
    }
    function g() public pure returns (string memory){
        return "method g()";
    }
}
```

# Configuring migration scripts

Configuring `./migrations/2_deploy_contracts.js` as follows:

```javascript
var Test = artifacts.require("./Test.sol");
var Migrations = artifacts.require("./Migrations.sol");

module.exports = function(deployer) {
  deployer.deploy(Test);
  deployer.deploy(Migrations);
};
```
The deploy function in this file also supports the contract constructor arguments.

# Configuring compilation and deployment parameters

The `./tronbox.js` file holds configurations the contract will deploy to. To specify the network, use --network NETWORK_NAME when migrating or testing.

```javascript
module.exports = {
  networks: {
      development: {

          from: 'some address',
          privateKey: 'some private key',
          consume_user_resource_percent: 30,
          fee_limit: 100000000,
          fullNode: "https://api.trongrid.io",
          solidityNode: "https://api.trongrid.io",
          eventServer:  "it is optional",
          network_id: "*" // Match any network id
      },
      production: {
          from: 'some other address',
          privateKey: 'some other private key',
          consume_user_resource_percent: 30,
          fee_limit: 100000000,
          fullNode: "https://api.trongrid.io",
          solidityNode: "https://api.trongrid.io",
          eventServer:  "it is optional",
          network_id: "*" // Match any network id
      },
    // ..... you can define other network configuration as well
  }
};
```

### Solidity Support
To set up a specific compiler version, using the networks.compilers.solc.version property. For example:

```js
module.exports = {
  networks: {
    // ...
    compilers: {
      solc: {
        version: '0.5.15' // for compiler version
      }
    }
  },

  // solc compiler optimize
  solc: {
    optimizer: {
      enabled: false, // default: false, true: enable solc optimize
      runs: 200
    },
    evmVersion: 'istanbul'
  }
}
```
Solidity compiler versions supported by TronBox:

- 0.4.24
- 0.4.25
- 0.5.4
- 0.5.8
- 0.5.9
- 0.5.10
- 0.5.12
- 0.5.13
- 0.5.14
- 0.5.15

# Contract Compiling

To compile the contract, use:
```shell
tronbox compile
```

By default, tronbox compiler only compiles modified contracts since last compile, to reduce unnecessary compiling. If you wish to compile the entire file, you can use `--all`.
```shell
tronbox compile --all
```

The compile output is in `./build/contracts` directory. If the directory doesn't exist, it will be auto-generated.

# Contract Deployment

Script migration is with a JavaScript file composed to broadcast onto Tron network.

The script migration caches your broadcast responsibility. Its existence is based on your broadcast needs and will adjust accordingly. 
When your work incurs significant change, the migration script you created will propagate the changes through the blockchain. 
Previous migration history records will undergo a unique Migrations contract to be recorded on the blockchain. Below is a detailed instruction.

To initiate the migration, use the following command:
```shell
tronbox migrate
```
This command will initiate all migration scripts within the `./migration` directory. If your previous migration was successful, `tronbox migrate` will initiate a new migration and use the `development` network by default.

If there is no new migration script, this command will have no operation. Instead, you can use --reset to re-deploy. You can also use --network NETWORK_NAME to specify a network. Eg. tronbox migrate `--reset --network production`

```shell
PS  C:\**\bare-box> tronbox migrate --reset --network production
Using network 'production'.
Running migration: 1_initial_migration.js
  Deploying Migrations...
  Migrations: 41271233ac2eea178ec52f1aea64627630403c67ce
  Deploying Test...
  Test: 41477f693ae6f691daf7d399ee61c32832c0314871
Saving successful migration to network...
Saving artifacts...
```
# Contract Trigger

The testing scripts are in the `./tests` directory. TronBox will ignore all other extensions except for `.js`, `.es`, `.es6`, and `.jsx`.

Below is an example testing script for test.js:

```js
var Test = artifacts.require("./Test.sol");
contract('Test', function(accounts) {
        it("call method g", function() {
            Test.deployed().then(function(instance) {
                  return instance.call('g');
                }).then(function(result) {
                  assert.equal("method g()", result, "is not call method g");
            });
        });
        it("call method f", function() {
            Test.deployed().then(function(instance) {
                  return instance.call('f');
                }).then(function(result) {
                  assert.equal("method f()", result, "is not call method f");
                });
        });
});
```

Running Testing Script:

```shell
PS C:\**\bare-box> tronbox test ./test/test.js 
Using network 'production'.
  Contract: Test
    √ call method g
    √ call method f
  2 passing (23ms)
```

# Project Boilerplates

It can be obtained by the following command:
```shell
tronbox unbox metacoin
```
It can also be found at [here](https://github.com/sullof/metacoin-box).