Consensus Algorithm
===================

The objective of all the blockchain protocols is to replicate a blockchain and
its associated state across the participating nodes. To achieve such an
agreement, each blockchain protocol deploys a consensus algorithm.


The public Aergo network uses **Delegated Proof of Stake (DPoS)** for
blockchain replication.

BP Election
    In DPoS, blocks are generated only by a limited number of nodes called
    Block Producers (BPs). BPs are elected via voting, where the voting power
    is weighted by staked tokens.

    BPs are re-elected round by round (blocks per round = the total number of
    the BPs). In each round, time is split into slots and each slot is
    assigned to one of the elected BPs. Only the permitted BP can produce a
    block in a time slot.

Staking & Voting
    Staking means locking up one's tokens for a minimum period of time. Any
    user wanting to vote must stake his tokens since the voting power is
    weighted by a staked tokens as remarked above.

    Voting is performed via a transaction. The voting results does not come
    into effect, immediately. The current BPs are elected based on the voting
    result gathered at the block number: (<current block number> / <the total
    number of BPs> - 1) * <the total number of BPs>.

    In other words, the voting results gathered in the past (approximately 1
    round before) are used for stability (recent blocks may be roll-backed via a
    reorganization).

Last Irreversible Block (LIB)
    In some blockchain protocols, a blockchain may branch into two or more, which is so called
    fork. Later, only one of them is chosen as the main branch via a
    set of rules defined by the blockchain protocol. Such a blockchain
    reorganization limits each block's finality and, in turn, transaction's
    finality. For example, a transaction, included in a block at one time, can
    be later rejected into the mempool.

    DPoS also allows blockchain fork. However a block becomes a last
    irreversible block (LIB) when it is (double time) confirmed by a majority (2/3+) of
    BPs. Once a block is determined as a LIB, it is not rollbacked so that it has
    finality.

