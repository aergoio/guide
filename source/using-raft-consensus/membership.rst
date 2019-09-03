Dynamic Membership Changement
=============================

Blockchain that uses the RAFT consensus can add or remove members dynamically through enterprise transactions. You do not have to restart running nodes or change the config file at this time.  
For a description of enterprise transactions, see `transacion-types <../specs/transactions.html#transaction-types>`__.

This article assumes the Aergo blockchain using raft consensus consisting of 3 nodes.

Abstaction
----------

.. note::

	**Membership changes must be made after the completion of one.** 
	When adding a new node, do not add another node again until blockchain synchronization is over, since the new node becomes unavailable until the chain is synchronized and up to date. If more than half of the nodes are out of sync, the cluster stops. 

Validation
^^^^^^^^^^
When executing a membership change transaction, leader node checks whether it is executable on the server.

- Case adding a new node
    
  If any node is not properly synchronized in the whole network, it will fail. The node is either in a shutdown state or a state where synchronization is slowed down due to a failure.

- Case removing a node

  If the cluster can be stopped when the node is excluded, the delete request will fail. Deletion to a failed node always succeeds.

Prework
^^^^^^^^^
You need set up and unlock :code:`ENTERPRISE ADMIN ACCOUNT` to execute commands with :code:`Enterprise Transaction`. See the page `transacion-types <../specs/transactions.html#transaction-types>`__.

Add Member
----------
Execute Add Node command
^^^^^^^^^^^^^^^^^^^^^^^^

Send an enterprise transaction to one of the cluster nodes.

.. code-block:: shell

    aergocli -p [RPC PORT] contract call --governance [ADMIN ACCOUNT] aergo.enterprise changeCluster [ { "command": "add", "name": "[NODE NAME]", "address": "[PEER ADDRESS]", "peerid":"[PEER ID]" } ]

    example> aergocli -p 10001 contract call --governance AmQJ6HTxMzq5eios54xubENE2wAe4puF3GDuaFxBzXkWa4KpfnWr aergo.enterprise changeCluster [ { "command": "add", "name": "raft4", "address": "/ip4/127.0.0.1/tcp/11004", "peerid":"16Uiu2HAmQti7HLHC9rXqkeABtauv2YsCPG3Uo1WLqbXmbuxpbjmF" } ]


Check the command result
^^^^^^^^^^^^^^^^^^^^^^^^
Query enterprise transaction results on one of the cluster nodes. The hash of enterprise transaction must be given as an argument.

.. code-block:: shell

  aergocli -p [RPC PORT] enterprise tx "[ENTERPRISE TX HASH]" --timeout=[VALUE IN SECONDS]


The changed cluster status can be checked using :code:`getconsensusinfo` RPC. :code:`"Total"` represents the total number of nodes and :code:`"Bps"` contains the block producer list.

.. code-block:: shell

    aergocli -p 10001 getconsensusinfo

Write configuration for a new node (new node only)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In the config file of the node to add, set the peer addresses of the block producers to the [p2p] :code:`npaddpeers`. If possible, register the peer address of all members currently registered in the raft cluster. However if necessary, only one connectable node can be set.

.. code-block:: shell

    [p2p]
    ...
    npaddpeers = [
    "/ip4/[IP ADDRESS FROM BP 01]/tcp/7846/p2p/[PEER ID FROM BP 01]",
    "/ip4/[IP ADDRESS FROM BP 02]/tcp/7846/p2p/[PEER ID FROM BP 02]",
    "/ip4/[IP ADDRESS FROM BP 03]/tcp/7846/p2p/[PEER ID FROM BP 03]"
    ]
    ...

    [consensus.raft]
    newcluster=false
    name="[NICKNAME OF NEW BP]"

Running
^^^^^^^
See the page `Configuring a Network <../running-node/configure-network.html#running>`__ to start a new node.

.. note::

    You should wait for the added node to finish synchronizing with the existing blockchain.

- Check if the new node recognized the raft leader node

	If ConsensusInfo.Status.Leader is set to one of the node names, the leader node is found.

    .. code-block:: shell

        aergocli -p 10004 blockchain
        > {"Hash":"3ivEw2o97WkpEvarBkVEYWCAQXhQ7BpHwmzs2L4WXUzJ","Height":7302,"ConsensusInfo":{"Type":"raft","Status":{"Leader":"raft1","Total":3,"Name":"raft4","RaftId":"dd44cf1a06727dc5","Status":null}},"ChainIdHash":"A9gpZUsFS9BVEfpUy6gbJ7YtGuHsYAmzamWkhDo54cak"}

- Make sure the BlockChain Height of the new node matches the Leader

    .. code-block:: shell

        aergocli -p [RPC PORT OF LEADER] blockchain
        aergocli -p [RPC PORT OF NEW NODE] blockchain


Remove Member
-------------

**Get RAFT ID to remove**

To delete a node, you should get RAFT ID of the node. RAFT ID is a value assigned after a node is added to RAFT. It is a value to idenity each node in RAFT Consensus.

You can get it in the result of blockchain RPC. In the example above, `dd44cf1a06727dc5` is the Raft ID of the "raft4" node.

.. code-block:: shell

 aergocli -p 10004 blockchain
 > {"Hash":"...","Height":7302,"ConsensusInfo":{"Type":"raft","Status":{"Leader":"raft1","Total":4,"Name":"raft4","RaftId":"dd44cf1a06727dc5","Status":null}},"ChainIdHash":"..."}


**Run enterprise transaction**

.. code-block:: shell

    aergocli -p [RPC PORT] contract call --governance [ADMIN ACCOUNT] aergo.enterprise changeCluster [ { "command": "remove", "id": "[RAFT ID]" } ]

    example> aergocli -p 10001 contract call --governance AmQJ6HTxMzq5eios54xubENE2wAe4puF3GDuaFxBzXkWa4KpfnWr aergo.enterprise changeCluster '[ { "command": "remove", "id": "dd44cf1a06727dc5" } ]'


**Check result of enterprise transaction**

As with adding nodes, you should check "Total" and "Bps" in the results of the getconsensusinfo RPC.

.. code-block:: shell

    aergocli -p 10001 getconsensusinfo

**Wait shutdown removed node**

If Enterprise Transaction is executed on the node to be deleted, the node terminates itself.

Add Member with Backup
----------------------

After adding a new node, it takes a long time to synchronize the existing blockchain. To reduce this time, you can add a new node by using backup data files obtained from existing member nodes. 
There is no conflict because the new node resets all the cluster-related information from the given backup data files at the first startup.

**Prepare backup datafiles**

The bakcup datafiles can be obtained by shutting down an existing node and copying the data directory.

.. note::

	Since the blockchain is already created in the data file, there is no need to create the genesis block.

**Write Configuration file**

When using backup data files, :code:`usebackup` must be set to true. Set the backup data directory to :code:`datadir` field.

.. code-block:: shell

    datadir="[copied data directory]"

    [p2p]
    ...
    npaddpeers = [
    "/ip4/[IP ADDRESS FROM BP 01]/tcp/7846/p2p/[PEER ID FROM BP 01]",
    "/ip4/[IP ADDRESS FROM BP 02]/tcp/7846/p2p/[PEER ID FROM BP 02]",
    "/ip4/[IP ADDRESS FROM BP 03]/tcp/7846/p2p/[PEER ID FROM BP 03]"
    ]
    ...

    [consensus.raft]
    newcluster=false
	usebackup=true
    name="[NICKNAME OF NEW BP]"

The rest of the process is the same as adding members without using backup.
