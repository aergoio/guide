Blocks
======

Block header
------------

=================  ============  ================================================================================================
Field                Type        Description
=================  ============  ================================================================================================
ChainID              []byte        identifier of chain
PrevBlockHash        []byte        hash of previous block
BlockNo              uint64        height of block
Timestamp            int64         block timestamp
BlocksRootHash       []byte        root hash of State SMT(sparse merkle tree)
TxsRootHash          []byte        Merkle root of transactions
ReceiptsRootHash     []byte        Merkle root of transaction receipts
Confirms             uint64        indicates how many block is confirmed by block in DPOS consensus
PubKey               []byte        public key of the block producer (BP)
CoinbaseAccount      []byte        the account address to which block reward is given
Sign                 []byte        BP's Signature for block header
=================  ============  ================================================================================================

ChainID
^^^^^^^
It is used to prevent the wrong block transmission from other chains.

=================  ============  ================================================================================================
Field                Type        Description
=================  ============  ================================================================================================
Version             int32           version number of chain
PublicNet           bool            true if public net
MainNet             bool            true if main net
CoinbaseFee         string          Fee consumed per tx. Fixed value
Magic               string          Magic string, arbitrarily chosen by each blockchain
Consensus           string          dpos or sbp
=================  ============  ================================================================================================

Version
^^^^^^^
Version is used to identify when the block format changes or when the function of the chain changes.

PublicNet
^^^^^^^^^
Differentiate between public and private networks.

MainNet
^^^^^^^
Differentiate between main net and other test net or other use net.

CoinbaseFee
^^^^^^^^^^^
CoinbaseFee is used to set the peak per Tx.

Magic
^^^^^
Magic can be considered as a name of a blockchain. The two blockchains with different magic strings reject each other's blocks (they are seperate, indenpendent blockchains).

Consensus
^^^^^^^^^
Specify the concensus method name used in the chain.


Block body
----------

=================  ============  ================================================================================================
Field                Type        Description
=================  ============  ================================================================================================
Txs                  []Tx         Transactions
=================  ============  ================================================================================================

