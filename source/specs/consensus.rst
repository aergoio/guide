Consensus Algorithm
===================

The public Aergo network uses **Delegated Proof of Stake (DPoS)**. This article explains some of the main concepts and their implementation in Aergo.

**Block Producer (BP):**
In Aergo, only a limited number of nodes generate blocks.
They are called Block Producers.
BPs are elected via users' voting, where the voting power is weighted by each voter's staked tokens.
Based on this voting, BPs are re-elected round by round so that they can be changed over time.
*The number of BPs and the number of stand-by BPs (candidates) have not been decided yet.*

**Staking:**
Before voting, a user must stake one's tokens.
*The detailed policies about staking/untaking has not been decided, yet. Policies include the minimum amount of staking, the mandatary days of staking, etc.*

**Voting:**
Any user with staked tokens can vote for BPs using a voting transaction.
The current BPs are elected based on the voting result gathered at the block number:
(<current block number> / <the total number of BPs> - 1) * <the total number of BPs>.
