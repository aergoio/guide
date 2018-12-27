Introduction
============

AERGO is an open platform that allows businesses to build innovative applications and services by sharing data on a trustless and distributed IT ecosystem.


Official Chain Software of Aergo Protocol

We are developing the most practical and powerful platform for blockchain businesses. This will be a huge challenge. There are 4 main ideologies regarding this project.

* Developer-friendly
* Guaranteed performance
* Scalable architecture
* Connect with the world

Roadmap
-------

These features are under active development but not quite ready for general use.

beginning: Skeleton (31, July, 2018)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Platform framework
* Stub consensus(dpos without voting)
* Account model
* Mempool
* Networking - p2p/protocol
* Cmd aergocli/aergosvr
* Simple client API
* Smart contract will not be released - you can see the prototype in [coinstack3sp2](https://github.com/coinstack/coinstackd)

1st: Aergo Alpha (31, Oct, 2018)
~~~~~
* Consensus - BFT-dPOS (election not integrated)
    * We provide BFT by solving various problems that may occur in dpos.
* Aergo SQL smart contract (Lua-jit)
    * It is a powerful smart contract language providing DB function.
* Client - Ship
    * Client framework and development environment
    * Provides a package management and testing environment similar to NPM.
* Client SDK
    * heraj (java)
    * herajs (javascript)
    * herapy (python)
* Browser Wallet (1~2 weeks later)
    * Chrome Extension provides a coin transfer wallet.
* Sub Project
    * Litetree
        * Improved SQLite is used to provide DB functionality in a block chain.
        * Provides higher performance through LMDB.
    * Sparse Merkle Tree
        * Provides fast, space-saving sparse merkle tree.
    * Pre-Testnet
        * Launch the pre-testnet to monitor operation environment.
        * We provide https://aergoscan.io.

2nd: Aergo Testnet (28, Dec, 2018)
~~~~
* BFT-dPOS with Voting
    * The pre-test net has the function of agreeing blocks among the set BPs. TestNet has a function to select BP through voting.
* Named Account
    * For user's convenience, Named Account function that can be accessed based on Name rather than Address is provided.
* Expanded Aergo Lua
    * The Aergo Lua feature has been extended for more convenient development.
* Advanced Client Framework
    * Provides a wallet interface that interacts with keystore and manages nonce.
    * Provides the ability to make smart contracts through interface calls.
    * Provides a contract library to issue tokens based on Aergo.
* Hub Enterprise
    * Enterprise customers view management and monitoring of their networks as a prerequisite.
    * We provide Hub Enterprise control solution to solve this problem.
* Merkle Bridge Verification
    * StateTrie Merkle proof verifications and delegated token transfers are now implemented in the merkle-bridge.
* Various Smart Contract Examples
    * Provide some standard smart contracts
* TestNet
    * Launch the Test Network to provide network for community and experment
    * We provide https://aergoscan.io.

3rd: Aergo Mainnet (planned in March, 2019)
~~~~
* Parallelism (inter-contract)
* Simple branching (2WP or simple Plasma)

4th: Aergo World Launch (planned in 4Q, 2019)
~~~~
* Orchestration with Aergo Horde
* Service with Aergo Hub
* Advanced performance features

5th: Aergo Future
~~~~
* Will be updated
