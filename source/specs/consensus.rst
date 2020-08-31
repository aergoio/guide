합의 알고리즘
=============

블록체인 프로토콜의 궁극적인 목적은 네트워크에 참여하는 모든 노드가 동일한
버젼의 블록체인을 공유하도록 하는 것이다. 이것을 위해서는 각 높이별 블록에 대해
블록을 생성하는 노드들 간의 동의가 필요하다. 이 동의를 이루는 과정에서 사용되는
것이 합의 알고리즘이다.

현재 아르고 퍼블릭 블록체인 네트워크는 블록체인에 대한 합의를 위해 **Delegated
Proof of Stake (DPoS)** 를 사용한다. DPoS에서는 블록을 생성하는 노드, 즉 블록 
생성자(Block Producer, BP)가 투표를 통해 선출되며 투표 결과는 모두 블록체인 상의
기록된다. 또 일정한 기준을 충족하는 블록은 최종 비가역적 블록(Last Irreversible 
Block, LIB)이 되어 최종성이 보장된다.


BP 선출

    DPoS에서 블록은 오직 제한된 수의 노드에 의해 생성된다. 블록 생성자(Block Producers, BP)는 지분 소유자(staker)에 의해 선출되는데, 지분 소유자가 되기 위해서는 최소 10,000 아르고 이상을 staking해야 한다. (staking 용 트랜잭션을 통해 수행)

    BP는 매라운드 단위(한 라운드는 100블록에 해당)로 재선출 된다. 각 라운드에서 블록은 매초 1개 생성된는데, 각 BP는 라운 내에 자신이 블록을 생성해야 할 고유의 순번이 할당되어 있어 특정 구간에서는 단 한개의 BP만 블록을 생성하게 된다.

스테이킹과 투표

    스테이킹이란 토큰을 일정한 시간 동안 블록체인 상에 묶어두는(lock-up) 것을 말한다. BP 선출과 같은 블록체인의 운영과 관련되 투표를 하고 싶다면 사용자는 자신의 토큰을 스테이킹해야 한다. 또 표수(voting power)는 스테이킹한 양에 비례한다.
    
    모든 과정은 트랜잭션을 통해 블록체인 상에서 이루어진다. 따라서 스테이킹과 투표의 전과정은 블록체인 상에 투명하게 기록기며 누구든 검증할 수 있다.
    
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

