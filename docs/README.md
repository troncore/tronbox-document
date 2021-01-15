# Introduction

TronBox is a development environment and framework for blockchains using the TRON Virtual Machine. TronBox provides the following core functionalities:
- Smart contract compilation
- Migration (deployment on the network)
- Testing

# Installation

OS requirement:

- NodeJS 5.0+
- Windows, Linux, or Mac OS X

```shell
npm install -g tronbox
```

> Note on Windows:  
> For Windows, the best way to run TronBox is to install an Ubuntu subsystem: https://docs.microsoft.com/en-us/windows/wsl/install-win10. After running Ubuntu from the prompt, proceed as usual. It is not recommended to run TronBox from Powershell.

# Initialize a Tron-Box Project

Enter the following command under an empty folder:
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
|  Directory                  |  Description  |
|  ----                 | ----  |
| ./contracts           | The directory storing all smart contract files. |
| ./migrations          | The directory storing all javascript files for migration. |
| ./test                | The directory storing all test scripts for testing the smart contract. |
| ./tronbox.js          | The configuration file of the project. Declare your Full Node address and Event Server in this file. |

# Basic Commands

| Command | Usage |
| ---- | ---- |
| tronbox compile           | Compiles all the smart contracts. The compiled result is stored into `./build/contracts`.<br/> This command only compiles files that have been modified since the last compile. |
| tronbox compile --all     | Re-compiles all the smart contracts. |
| tronbox migrate           | Deploys the contract. This command only migrates changes since the last successful migration. |
| tronbox migrate --reset   | Re-migrates all the smart contracts. |
| tronbox test [test_script_path]   | Runs all test scripts. Test file name definition is optional. It also can be run with --reset flag. |
| tronbox console   | The console supports the tronbox command. <br/> For example, you can invoke migrate --reset in the console. The result is the same as invoking tronbox migrate --reset in the command. |

Extra Features in TronBox Console:
- All the compiled contracts can be used, just like in development & test, front-end code, or during script migration.
- After each command, the contract is re-loaded. After invoking the migrate--reset command, you can immediately use the new address and binary.
- Every returned command's promise is automatically logged. There is no need to use then(), which simplifies the command.

# Verifying the PGP signature

Prepare, you need to install the npm pkgsign for verifying.

First, get the version of tronbox dist.tarball:
```shell
$ npm view tronbox dist.tarball
https://registry.npmjs.org/tronbox/-/tronbox-2.7.17.tgz
```

Second, get the tarball:
```shell
$ wget https://registry.npmjs.org/tronbox/-/tronbox-2.7.17.tgz
```

Finally, verify the tarball：
```shell
$ pkgsign verify tronbox-2.7.17.tgz --package-name tronbox
extracting unsigned tarball...
building file list...
verifying package...
package is trusted
```

You can find the public [here](https://keybase.io/tronbox/pgp_keys.asc)。

# CHANGELOG

Last stable version：2.7.17   
More changelogs, see [here](https://github.com/tronprotocol/tronbox/blob/master/CHANGELOG.md).
