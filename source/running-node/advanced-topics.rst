Advanced Topics
===============


Genesis block configuration
---------------------------

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

Manually creating peer identification
-------------------------------------

Every peer (i.e. instance of aergosvr) is identified by a peer id, derived from its public key.
If unspecified, aergosvr creates these automatically, but you may want to do it manually to control the generation of keys and make backups.
You can use the CLI to do that:

.. code:: shell

    aergocli keygen your-keyname

    # Using Docker:
    docker run --rm -v $(pwd):/tools/ aergo/tools aergocli keygen your-keyname

You then need to specify the path to the generated your-keyname.key file in the Aergo configuration:

.. code:: 

   ...
   [p2p]
   npkey = "/aergo/your-keyname.key"
   ...
