Block spec
==========

block header
------------------

=================  ============  ================================================================================================
Field                Type        Description
=================  ============  ================================================================================================
ChainID              []byte        identifier of chain
PrevBlockHash        []byte        hash of previous block
BlockNo              uint64        height of block
Timestamp            int64         generated timestamp by BP
BlocksRootHash       []byte        root hash of State SMT(sparse merkle tree)
TxsRootHash          []byte        Merkle root of transactions
ReceiptsRootHash     []byte        Merkle root of transaction receipts
Confirms             uint64        indicates how many block is confirmed by block in DPOS consensus
PubKey               []byte        public key of BP that generated the block
CoinbaseAccount      []byte        account address of BP that generaged the block to receive fee for transactions
Sign                 []byte        BP's Signature for block header
=================  ============  ================================================================================================

ChainID
^^^^^^^
chain을 idenify하는 값으로 다른 chain으로 부터 잘못된 Block전송을 막기 위해 용도로 사용된다.

=================  ============  ================================================================================================
Field                Type        Description
=================  ============  ================================================================================================
Version             int32           chain의 version number
PublicNet           bool            true if public net
MainNet             bool            true if main net
CoinbaseFee         string          tx 당 소모되는 fee로서 고정값
Magic               string          chain verify를 위한 magic number
Consensus           string          dpos or sbp
=================  ============  ================================================================================================

Version
^^^^^^^
chain의 feature변경이나 block format이 변경 될때 이를 구분하기 위해 사용된다.

PublicNet
^^^^^^^^^
public net인지 private net인지를 구분한다.

MainNet
^^^^^^^
main net인지 기타 test net 혹은 다른 용도의 net인지를 구분한다.

CoinbaseFee
^^^^^^^^^^^
Tx당 고정으로 소모되는 fee를 설정하기 위해 사용한다.

Magic
^^^^^
ChainID의 verify를 위해 사용

Consensus
^^^^^^^^^
해당 Chain에서 사용되는 consensus method name을 지정한다.


Block body
------------------

=================  ============  ================================================================================================
Field                Type        Description
=================  ============  ================================================================================================
Txs                  []Tx         Transactions
=================  ============  ================================================================================================

Block에 포함된 transaction들을 저장한다.
Transaction 구조는 Techspec/transaction 을 참고한다.

