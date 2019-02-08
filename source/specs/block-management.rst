Block Management
================

This article describes implementation details of management of blocks in the Aergo server.
It is aimed at blockchain server developers.

Block generation
----------------

The block factory is responsible for generating blocks and part of the consensus engine.

The consensus engine manages when blocks are created on which nodes.
AERGO uses DPoS by default. A new block producer is selected every second.

The process for creating blocks in the block factory is as follows.

#. Select transactions from from mempool
#. Perform a transaction one by one. If the execution is successful, the transaction is included in the block.
#. If the block size and time limit are exceeded, the transaction is no longer included.
#. Create blocks that contain executed transactions and add signatures to blocks.

The block factory sends the generated blocks to the chain service for propagation and storage.

Notify block 
------------

Blocks passed to the chain service are propagated to the connected peer nodes and stored in the database.

The chain service of a node that received the block verifies the block, executes the transactions, and adds the block to its own database.

Save block
----------

The chain service stores the generated blocks in the DB for permanent storage. 
It also stores additional meta information to store connection information about the chain.

AERGO uses the Badger database as its permanent storage.
Badger stores data in key/value form and supports the concept of transactions similar to a typical database.
