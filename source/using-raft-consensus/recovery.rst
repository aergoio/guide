Recovery
========
If more than half of the cluster's nodes become unrecoverable, the raft cluster can no longer delete or add any node. This is because more than half of nodes cannot agree to the change.

In this case, if you want to keep the data of BlockChain as it is, you can create a new cluster by using backup of existing node data. However, RAFT cluster information cannot be recycled.

This is called recovery.

There are some differences between recovery and general cluster creation.

1. When creating a cluster using the recovery function, there must be one initial member node.
2. Since genesis block is already created in blockchain, you should set initial node in config file instead of genesis block.
3. If you want to configure N-node clusters, you need to add members dynamically from the second member.

The cluster related information is deleted in the backup files. The newly created cluster creates a block following blockchain that has already created. 

Prepare backup datafiles
------------------------

Copy backup datafiles from the old node to storage on the new node.

Write Configuration file(only initial node)
-------------------------------------------

.. code-block:: shell
    datadir="[copied data directory]"

    [p2p]
    ...
    npaddpeers = [
    ]
    ...

    [consensus.raft]
    newcluster=true
    usebackup=true
    name="[NICKNAME OF NEW BP]"
    recoverbp={name="[NICKNAME OF NEW BP]",address="/ip4/[IP ADDRESS FROM BP 01]/tcp/7846",peerid="[PEER ID FROM NEW BP 01]"}

- To create a new cluster, you need to set :code:`newcluster`=true.
- Set :code:`usebackup`=true to use backup data files. 
- Set the member of the new cluster to :code:`recoverbp`. At this time, member must only be one node.

Running a node
--------------
Start the node. After that, add nodes one by one using Dynamic membership change.

