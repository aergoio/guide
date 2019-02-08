Configuration
=============

This page explains the possible ways to configure an Aergo node.

There are three common modes of operation for aergo server:

* BP Node - Block provider node, which mines and propagates blocks
* Full Node - Node to get and validate all blocks
* Single Node - BP Node without peer, which uses to something like test.

We describe only single node configuration in this. More will updated for the BP Node and full node.

## Single Aergo Server (default)

We will show default configuration file next. You can check the each fields' meaning in [here](https://github.com/aergoio/aergo/wiki/server_configuration)

```toml
# aergo TOML Configuration File (https://github.com/toml-lang/toml)
# base configurations
datadir = ".aergo/data"
enableprofile = false
profileport = 6060
enablerest = false
enabletestmode = false
authdir = ".aergo/auth"

[rpc]
netserviceaddr = "127.0.0.1"
netserviceport = 7845
nstls = false
nscert = ""
nskey = ""
nsallowcors = false

[rest]
restport = "8080"

[p2p]
# Set address and port to which the inbound peers connect, and don't set loopback address or private network unless used in local network 
netprotocoladdr = "" 
netprotocolport = 7846
npbindaddr = ""
npbindport = -1 
# Set file path of key file
npkey = ""
npaddpeers = [
]
nphiddenpeers = [
]
npmaxpeers = "100"
nppeerpool = "100"
npexposeself = true
npusepolaris = true
npaddpolarises = [
]

[blockchain]
# blockchain configurations
maxblocksize = 1048576

[mempool]
showmetrics = false
dumpfilepath = ".aergo/mempool.dump"

[consensus]
enablebp = true
enabledpos = false
blockinterval = 1
dposbps = 23
bpids = [
]
```

## Testmode

To enable testmode, either pass the command line option `--testmode` to aergosvr or set `enabletestmode = true` in the configuration.

In testmode, all new accounts are assigned a high number of Aergo tokens by default, basically circumventing balance checks. This means you can send any transaction without first pre-funding hard-coded accounts using a genesis block.

Testmode **MUST NOT** be used in production.

## Reference

| Module        | Property name    | Meaning                                  |
| ------------- |------------------|------------------------------------------|
| (default)     | datadir          | data files' base path                    |
|               | enableprofile    | whether use profie                       |
|               | profileport      | profile port                             |
|               | enablerest       | whether use REST API                     |
|               | enabletestmode   | whether test mode                        |
|               | personal         | enable personal account API              |
| rpc           | netserviceaddr   | rpc hostname or address                  |
|               | netserviceport   | rpc port number                          |
|               | nstls            | whether applying tls to rpc              |
|               | nscert           | certificate file path for rpc tls        |
|               | nskey            | key file path for rpc tls                |
|               | nsallowcors      | whether allows CORS                      |
| rest          | restport         | port number for REST API                 |
| p2p           | netprotocoladdr  | ip address or domain name to which other peers connect |
|               | netprotocolport  | port number for connect                  |
|               | npbindaddr       | listener binding address                 |
|               | npbindport       | listener binding port                    |
|               | npkey            | key file path for p2p tls                |
|               | npaddpeers       | initial peer list on start-up            |
|               | nphiddenpeers    | peerid list which will not inform to other peers |
|               | npexposeself     | whether to advertise node to `Polaris <../tools/polaris.html>`__ or other peers. |
|               | npusepolaris     | whether to connect Polaris for finding other peers    |
|               | npaddpolarises   | list of addresses of custom polaris      |
| blockchain    | maxblocksize     | maximum block size                       |
|               | coinbaseaccount  | address where is send rewards for mining |
| mempool       | showmetrics      | whether if turn periodic log on          |
|               | dumpfilepath     | file path for recording mempool at process termination |
|               | verifiers        | number of concurrency for sign verifying |
| consensus     | enablebp         | whether this node is BP                  |
|               | enabledpos       | whether consensus is dPoS                |
|               | blockinterval    | minging interval in seconds              |
|               | dposbps          | the number of BPs                        |
|               | bpids            | BP peer ids                              |

## Logging options

It is possible to customize the log output format of all Aergo CLI tools using a file called `arglog.toml` placed in the current working directory.

This file is specified [here](https://github.com/aergoio/aergo-lib/blob/fe30a6e424e5b963f3c4e9c0ea3da4c0c87f595b/log/log.go#L9-L40).

```toml
level = "info"  # default log level
formatter = "json"  # format: console, console_no_color, json
caller = true  # enabling source file and line printer
timefieldformat = "RFC3339"

[chain]
level = "info"  # optional, log level for 'chain' module

[dpos]
level = "info"

[p2p]
level = "info"

[consensus]
level = "info"

[mempool]
level = "info"

[contract]
level = "info"

[syncer]
level = "info"

[bp]
level = "info"
```
