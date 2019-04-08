Consensus Algorithm
===================

The objective of all blockchain protocols is to replicate a blockchain and
its associated state across participating nodes. To achieve such an
agreement, each blockchain protocol deploys a consensus algorithm.

The public Aergo network uses **Delegated Proof of Stake (DPoS)** for
blockchain consensus.

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
    user wanting to vote must stake their tokens since the voting power is
    weighted by the number of staked tokens, as remarked above.
    
    All of these requests are performed via a `transaction <./transactions.html#governance-type>`_.
    Therefore, all proccesses are transparently recorded in the blockchain
    and can be verified by anyone.
    
    After the voting transaction is included in the block, the results are 
    caculated immediately. But there is a slight delay until it is applied.
    The current BPs are elected based on the voting result gathered at 
    the block number: (<current block number> / <the total number of BPs> - 1) * <the total number of BPs>.

    In other words, the voting results gathered in the past (approximately 1
    round before) are used for stability (recent blocks may be roll-backed via a
    reorganization).

    Votes are locked for a certain period of time to prevent users from spamming
    votes. On the public Aergo network, this is currently approx. 1 day (after 60 * 60 * 24 block number).
    That means, after casting a vote, a user can only change their vote after 1 day has passed.
    
Last Irreversible Block (LIB)
    In some blockchain protocols, a blockchain may branch into two or more, which is called
    a fork. Later, only one of them is chosen as the main branch via a set of rules
    defined by the protocol. Such reorganizations limit each block's finality and,
    in turn, transaction's finality.
    For example, a transaction, included in a block at one time, might be
    rejected later.

    DPoS also allows blockchain forks. However, a block becomes a last
    irreversible block (LIB) when it is (double time) confirmed by a majority (2/3+) of
    BPs. Once a block is determined as a LIB, it cannot be rolled back, i.e. it
    achieves finality.

