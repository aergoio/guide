Consensus algorithm
===================

The public Aergo network uses **Delegated Proof of Stake (DPoS)**.

- Block Producer (BP): In Aergo, only a limited number of nodes generate blocks. They are called Block Producers. BPs are elected via users' voting among BP candidates. The voting power is weighted by staked tokens. The total number of BPs have not been decided, yet. BPs can be changed round by round. Currently each BP generate one block per round.

- Staking: Before voting, a user must stake one's tokens. The detailed policies about staking/untaking has not been decided, yet (for example, the minimum mount of staking and the mandatary days of staking).

- Voting: Any user having staked tokens can vote for BPs via voting transaction. The current BPs are elected based on the voting result gathered at the block number: (<current block number> / <the total number of BPs> - 1) * <the total number of BPs>.
