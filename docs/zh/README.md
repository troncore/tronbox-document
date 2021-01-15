# 介绍

TronBox是波场的智能合约开发、测试和部署工具。提供以下核心功能：
- 智能合约编制
- 迁移（在网络上部署）
- 测试

# 安装

OS 要求
- NodeJS 5.0+
- Windows, Linux, or Mac OS X

```shell
npm install -g tronbox
```

> #### Windows安装注意事项  
> 对于windows系统运行tronbox的最佳方法是安装Ubuntu子系统。不建议在Powershell中运行tronbox。

# 创建一个tronbox项目

在一个空的文件夹下输入以下命令
```shell
tronbox init
```

```shell
tree .
.
├── contracts
│   └── Migrations.sol
├── migrations
│   ├── 1_initial_migration.js
│   └── 2_deploy_contracts.js
├── sample-env
├── test
├── tronbox-config.js
└── tronbox.js
```
|  目录                  |  说明  |
|  ----                 | ----  |
| ./contracts           | 你所有的智能合约都在这里 |
| ./migrations          | 所有用于迁移的 javascript 文件都在这里 |
| ./test                | 持有所有测试脚本来测试您的智能合约 |
| ./tronbox.js          | 您项目的配置。在此文件中声明您的 fullnode 地址和事件服务器 |

# 基本命令

| 命令 | 用法 |
| ---- | ---- |
| tronbox compile           | 编译所有智能合约。结果进入 ./build/contracts。<br/> 它只会编译自上次编译以来被修改的文件。|
| tronbox compile --all     | 重新编译一切 |
| tronbox migrate           | 部署你的合同。它只会在上次成功迁移后进行迁移。|
| tronbox migrate --reset   | 重新迁移一切 |
| tronbox test [test_script_path]   | 运行所有测试脚本。定义测试文件名是可选的。它也可以使用 --reset 标志运行 |
| tronbox console   | 控制台支持 tronbox 命令。<br/> 例如: 您可以在控制台中调用 “migrate --reset”。结果与在命令中调用tronbox migrate --reset 相同。 |

TronBox Console的特点
- 可以使用所有已编译的合同，就像在开发 & 测试、前端代码或脚本迁移期间一样。
- 每个命令之后，您的合同将被重新加载。 在调用 migrate - rest 命令后，您可以立即使用新地址和二进制文件。
- 每个返回命令的承诺将自动记录。 不需要使用 then（），这将简化命令。

# Tronbox 安全性验证

通过签名工具 pkgsign npm包对tronbox进行签名验证，需要先安装pkgsign工具。  

安装完pkgsign后，需要先获取tronbox的签名包：
```shell
$ npm view tronbox dist.tarball
https://registry.npmjs.org/tronbox/-/tronbox-2.7.17.tgz
```

下载 tronbox tarball包：
```shell
$ wget https://registry.npmjs.org/tronbox/-/tronbox-2.7.17.tgz
```

最后，验证：
```shell
$ pkgsign verify tronbox-2.7.17.tgz --package-name tronbox
extracting unsigned tarball...
building file list...
verifying package...
package is trusted
```
签名使用的公钥地址[在这](https://keybase.io/tronbox/pgp_keys.asc)。

#### 更新日志

最新稳定版本：2.7.17 
