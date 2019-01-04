Introduction
============

**Hub Enterprise** is a framework that can manage and monitor **Aergo blockchain**
and the nodes that make up this blockchain for enterprise customer. 

Hub Enterprise provides a Web UI that makes it easier to manage block chains for users.

Hub Enterprise offers three key features:

- Managing Blockchain & Blockchain node
- Monitoring Blockchain & Blockchain node
- Deploying and Managing Smart Contract


Managing Blockchain and Blockchain Node
---------------------------------------
**Create blockchain**

The following user input arguments are required to create a blockchain:

- Name : Blockchain name
- bNodes : Number of nodes to be used for this blockchain

As many blockchain nodes as you enter are created, which run in one of a Docker container provided by the node provider.
Blockchain nodes in one blockchain are joined by peer to each other, and are fully connected. Peer for each blockchain node can be set and checked in the config file.

After you create the block chain, you can use the returned IDs of blockchain nodes to check the information of each node.
The Web address for viewing information on the blockchain node is as follows:

.. code-block:: text

    [Docker container IP]/bnode/[node ID]

Now you can use the blockchain in Hub Enterprise. You can manage and monitor the blockchain.


Monitoring Blockchain and Blockchain Node
-----------------------------------------
**Monitoring Blockchain**

You can monitor the created blockchain and monitor the following resources:

- Best Block for each Block Provider
- Transactions performed per minute
- CPU
- RAM
- Aergo log


.. todo::
   Deploying and Managing Smart Contract
