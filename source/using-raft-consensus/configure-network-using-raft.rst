Configuring a Network Using RAFT
================================

This article describes the process for setting up a network of multiple block producers using RAFT Consensus.
**RAFT consensus can only be used for private networks.**
A block chain network using RAFT consensus is called a RAFT Cluster.

All the settings below assume a case where a blockchain network consisting of 3 nodes using RAFT consensus.

To create a blockchain network using RAFT consensus, you need to configure the genesis block and config file.
Other settings are the same as the private network configuration using DPoS.


Common Configuration (per machines)
-----------------------------------

To configure aergo node as a block producer, it requires common configuration as descriped on `Configuring a Network <../running-node/configure-network.html#configuring-a-network>`__ page.

However, additional configuration requires in genesis block and config file.


Write configuration files for RAFT
-----------------------------------

The initial nodes of a new RAFT cluster should be added to the genesis block. Every node in the cluster is a candidate to be a block producer. Only one of these nodes becomes a leader to create a block. The remaining nodes become followers who receive instructions from the leader. If a leader crashes, one of the follower nodes becomes the new leader.

Nodes that cannot be block producers can be used by adding them as fullnodes. These nodes communicate via the P2P protocol but are not included in the RAFT cluster.

**genesis.json** (same for all machines)

- RAFT is not supported in the public network, so :code:`chain_id` should be set to public and :code:`maninet` should be set to false.
- Add the nodes to be included in the RAFT cluster to the :code:`enterprise_bps` entry. Add the :code:`name`, :code:`address`, and :code:`peerid` of the desired nodes to the enterprise_bps. The :code:`address` field uses the p2p peer format of the config file.
- set :code:`consensus` field to :code:`"raft"`.
- set the :code:`bps` entry to an empty list.

.. code-block:: json

    {
        "chain_id":{
            "magic": "[insert an identifier string for your network]",
            "public": false,
            "mainnet": false,
            "consensus": "raft"
        },
        "timestamp": 1548918000000000000,         
        "balance": {
            "[insert address from genesis]": "470000000000000000000000000",
            "[insert address from bp01]": "10000000000000000000000000",
            "[insert address from bp02]": "10000000000000000000000000",
            "[insert address from bp03]": "10000000000000000000000000"        },
        "bps": [
        ],
        "enterprise_bps": [
          {
            "name": "[NICKNAME OF BP 01]",
            "address": "/ip4/[IP ADDRESS FROM BP 01]/tcp/7846",
            "peerid": "[PEER ID FROM BP 01]",
          },
          {
            "name": "[NICKNAME OF BP 02]",
            "address": "/ip4/[IP ADDRESS FROM BP 02]/tcp/7846",
            "peerid": "[PEER ID FROM BP 02]",

          },
          {
            "name": "[NICKNAME OF BP 03]",
            "address": "/ip4/[IP ADDRESS FROM BP 03]/tcp/7846"
            "peerid": "[PEER ID FROM BP 03]"
          }
        ]
    }

**config.toml** (one per machine)

.. code-block:: toml

    # aergo TOML Configration File (https://github.com/toml-lang/toml)
    ...  insert common configuration for block producer ...

    [p2p]
    npaddpeers = [
    ]
    ...
    ...
    # configuration for block producer using RAFT
    [consensus.raft]
    newcluster=true
    name="[NICKNAME OF THIS BP]"


Basically, the settings when using DPoS should be applied to the config in the same way. But you don't need to set :code:`npaddpeers` of [p2p]. Nodes use the :code:`address` value of enterprise_bps to connect to others.

To use RAFT, :code:`[consensus.raft]` section should be added. :code:`name` is the current node's nickname. The name must be included in the enterprise_bps of the genesis block. When you create a new RAFT cluster, you must set :code:`newcluster`=true.

Other settings
---------------
**To change block creation cycle**

.. code-block:: toml
    
    [consensus]
    blockinterval=[insert value in seconds]

**To change heartbeat cycle**

.. code-block:: toml

  [consensus.raft]
  heartbeattick=[insert value in milliseconds ]

**To skip creating empty block**

.. code-block:: toml

  [consensus.raft]
  skipempty=true
