# 开始

使用tronbox部署智能合约需要先编写智能合约文件，再编译智能合约，最后脚本迁移并部署。之后可以使用脚本进行测试。

# 编写合约

所有的智能合约文件需要放在`./contacts`目录下。 默认会有一个后缀名是.sol的合约文件。但如果需要写智能合约就要创建新的文件。 

Tronbox 要求将智能合约名和文件名一样。 例如，如果文件名是 Test.sol，我们可以按如下方式编写合约：
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

# 配置迁移脚本

按如下方式配置 `./migrations/2_deploy_contracts.js`：

```javascript
var Test = artifacts.require("./Test.sol");
var Migrations = artifacts.require("./Migrations.sol");

module.exports = function(deployer) {
  deployer.deploy(Test);
  deployer.deploy(Migrations);
};
```
此文件中的deploy函数也支持合约构造函数传参。

# 配置编译和部署参数
`./tronbox.js` 文件保存网络信息和部署参数。要指定网络，请在迁移或测试时使用--network NETWORK_NAME 。

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

### Solidity 配置

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
TronBox支持的solidity版本如下：

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

# 编译合约

命令：
```shell
tronbox compile
```

默认情况下，tronbox 编译器仅编译自上次编译以来修改的合同，以减少不必要的编译。 
如果要编译整个文件，可以使用参数 `--all`。
```shell
tronbox compile --all
```

编译后有.json文件生成，位于./ build / contracts目录中。如果该目录不存在，则将自动生成该目录。

# 部署合约

此命令将调用migrations目录中的所有迁移脚本。  

如果先前的迁移成功，则tronbox迁移将调用新创建的迁移。 如果没有新的迁移脚本，此命令将没有任何作用。 相反，您可以使用选项--reset重新启动迁移脚本。 

命令：
```shell
tronbox migrate
```
重新迁移：
```shell
PS  C:\**\bare-box> tronbox migrate --reset 
Using network 'production'.
Running migration: 1_initial_migration.js
  Deploying Migrations...
  Migrations: 41271233ac2eea178ec52f1aea64627630403c67ce
  Deploying Test...
  Test: 41477f693ae6f691daf7d399ee61c32832c0314871
Saving successful migration to network...
Saving artifacts...
```
# 测试

测试脚本位于 `./tests` 目录中。 tronbox将忽略除.js，.es，.es6和.jsx之外的所有其他扩展。
 
下面是`./tests/test.js`的示例测试脚本：
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

运行测试脚本:

```shell
PS C:\**\bare-box> tronbox test ./test/test.js 
Using network 'production'.
  Contract: Test
    √ call method g
    √ call method f
  2 passing (23ms)
```

# 项目模版
可以通过以下命令获取:
```shell
tronbox unbox metacoin
```
也可以在[这里](https://github.com/sullof/metacoin-box).