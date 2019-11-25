Genesis block configuration
===========================

The genesis block is the first block created on a new chain according to a predefined specification.
If you want to bootstrap your own Aergo blockchain, you will need to configure and create the genesis block.
All nodes producing blocks and syncing with the network need to use the same genesis block.

You can find an example genesis configuration json file `here <https://github.com/aergoio/aergo-docker/blob/1ad16cf7881d9ba8f2efc350cf609c9416e76666/node/testnet-genesis.json>`_.
It is the one used for the public testnet.

=============  ======  ==========================================================================================
Key            Type    Explanation
=============  ======  ==========================================================================================
chain_id       obj
– magic        string  A name identifier used to distinguish different chains. Example: testnet.aergo.io
– public       bool    If this chain is for a public (true) or private network. This may affect certain features.
– mainnet      bool    If this is the main network (true) or a sidechain for another network.
– consensus    string  This blockchain's consensus type (dpos)
– coinbasefee  string  The default transaction fee amount 
timestamp      number  The genesis block's creation timestamp in nanoseconds
balance        obj     A mapping of addresses and allocated balances
bps            list    A list of fallback block producer peer ids.
=============  ======  ==========================================================================================
