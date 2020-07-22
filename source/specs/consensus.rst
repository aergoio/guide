합의 알고리즘
=============

블록체인 프로토콜의 궁극적인 목적은 네트워크에 참여하는 모든 노드가 동일한
버젼의 블록체인을 공유하도록 하는 것이다. 이것을 위해서는 각 높이별 블록에 대해
블록을 생성하는 노드들 간의 동의가 필요하다. 이 동의를 이루는 과정에서 사용되는
것이 합의 알고리즘이다.

현재 아르고 퍼블릭 블록체인 네트워크는 블록체인에 대한 합의를 위해 **Delegated
Proof of Stake (DPoS)** 를 사용한다. DPoS에서는 블록을 생성하는 노드들을 투표를 통해
선출한다. 또 일정한 기준을 충족하는 블록은 최종 비가역적 블록(Last Irreversible Block,
LIB)이 되어 최종성이 보장된다.


BP Election
    In DPoS, blocks are generated only by a limited number of nodes called
    Block Producers (BPs). BPs are elected via voting, where the voting power
    is weighted by staked tokens.

    BPs are re-elected round by round (blocks per round = 100). In each round,
    time is split into slots and each slot is assigned to one of the elected BPs.
    Only the permitted BP can produce a block in a time slot.

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
    the block number: (<current block number> / 100 - 1) * <the total number of BPs>.

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

