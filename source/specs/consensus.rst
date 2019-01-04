Consensus Algorithm
===================

The public Aergo network uses **Delegated Proof of Stake (DPoS)**. This article explains some of the main concepts and their implementation in Aergo.

Block Producer (BP)
    In Aergo, only a limited number of nodes generate blocks. They are called Block Producers.
    BPs are elected via users' voting, where the voting power is weighted by each voter's staked tokens.
    Based on this voting, BPs are re-elected round by round so that they can be changed over time.
    *The number of BPs and the number of stand-by BPs (candidates) have not been decided yet.*

Staking
    For the BP election, the voting power is weighted by a voter's staked tokens.
    Staking means locking up a number of one's tokens for a minimum period of time.
    *The detailed policy about staking/unstaking has not been decided yet. This includes the minimum amount of staking and the mandatory days of staking.*

Voting
    Any user with staked tokens can vote for BPs by using a voting transaction. But the voting results are not affected immediately.
    The current BPs are elected based on the voting result gathered at the block number:
    (<current block number> / <the total number of BPs> - 1) * <the total number of BPs>.
    In other words, the voting results gathered in the past (approximatedly 1 round before) are used for stability (recent blocks may be rollbacked via a reorganization).
