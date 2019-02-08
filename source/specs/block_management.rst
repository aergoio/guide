Block management
==========================

Generation of Block
------------------
The block factory of the constensus engine generates blocks.

The Consensus engine is a module that manages when blocks are created on which nodes.
Because AERGO uses DPOS as the default constants, BP turns around every second and generates blocks with the generated rights.

The process for creating blocks in the Block Factory is as follows.

#. Bring possible Transactional candidates from mempool.
#. Perform a transaction one by one and if the execution is successful, the transaction is included in the block.
#. If the block size and time limit are exceeded, the transaction is no longer included.
#. Create blocks that contain executed transactions and add signatures to blocks.

Block Factory sends the generated blocks to the ChainService for permanent storage.

ChainService stores block and additional meta information in internal storage.

Notify Block 
-----------------
The blocks passed to the Chain Service are propagated to the connected Peer Node before they are stored in the DB.

The chainService of the node that received the block verifies the block, executes the transactions, and adds the block to its chain.

Save Block
------------------
ChainService stores the generated blocks in the DB for permanent storage. 

The chain service stores additional meta information to store connection information about the chain.

AERGO uses the Badger Database as its permanent storage.

Badger stores data in key/value form and supports the concept of Transaction similar to a typical database.

Information stored in the DB is as follows.

The DB is managed in two separate categories, ChainDB and StateDB.
This article describes the ChainDB used to store chain information.

<TODO: For information on StateDB, see ./State>

Block info
^^^^^^^^^^
This information is used to search for blocks in a chain using a hash.
Typically, when a block is requested as a hash from another node, the Hash is searched for and responded to the block with a key.

=================  ==========================
Key					Value
=================  ==========================
Hash of Block       serialized data of Block
=================  ==========================



Chain info
^^^^^^^^^^
Chain info is the information that indicates when a block is connected to the main branch.

If the main branch is not connected, that is, only block info is saved and the chain info is not stored.

You can search the blocks of every height of the main branch using the chain info. You can know the height of the top block of the chain.

Chain info is also used to determine whether the block belongs to the main branch or the sub branch.

=================  ==================================
Key					Value
=================  ==================================
Number of Block     block hash
Latest block Key    latest block number of main branch
=================  ==================================


Transaction Info
^^^^^^^^^^^^^^^^^^^^
tx info is meta information of the transaction that was performed in the main branch.
If you search the DB with transaction hash, you can see which block the transaction is stored in and where the transaction is stored in the block.

The contents of the transaction are not saved separately. Therefore, in order to know the contents of the actual transaction, you must search Block info using Block hash and index found in tx info.

=================   ==============================================
Key					Value
=================   ==============================================
Transaction hash    (Block Hash, index of transaction in block)
=================   ==============================================
