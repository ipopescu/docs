---
slug: /concepts/economics/concepts
---

# Staking Concepts

The Casper Mainnet is a Proof-of-Stake blockchain that allows validators to stake the Casper native token CSPR on the network. Validators receive CSPR as an incentive for continuing to maintain and secure the network. CSPR rewards are distributed as blocks are validated into existence and organized into eras.

**Consensus mechanism:** The Casper Mainnet and Testnet use a Proof-of-Stake consensus mechanism called [Zug](../../design/zug.md). Another Casper network can choose between Zug and [Highway](../../design/highway.md) using the network's chainspec.

<!--TODO update the number of validators with 2.0 -->
**Number of validators:** The Casper Mainnet supports up to 250 validators on the network. This number is chosen to strike a balance between performance and decentralization. This platform parameter can be increased through upgrades as development continues and performance improves. In addition, validators can stake on the Casper Mainnet through a process of permission-less bonding by participating in an auction for the validator slot.

**Permission-less bonding:** For validators to begin staking and earning rewards, they must win a staking auction by competing with current and prospective validators to supply one of the forthcoming top stakes for a given era. This process is permission-less, meaning validators can join and leave the auction without restrictions, except for a waiting period to unlock staked tokens.

<!--TODO They do not receive rewards for current activity, but can still be rewarded for past signatures due to lookback, check with @fizyk20-->
**Unbonding:** To detach from the Casper Mainnet, it takes seven eras for both validators and delegators. Neither validators nor delegators receive rewards for the seven eras required for unbonding, as they are not actively contributing to the network's security during that time. However, during the unbonding period, they may receive rewards for having participated in past eras.

<!--TODO update block times with 2.0-->
**Eras and block times:** An era on the Casper Mainnet is roughly 2 hours long. Casper's Zug consensus allows validators to propose blocks as quickly as network conditions allow. We anticipate block times to last between 8 seconds and 1 minute.

**Block rewards:** Reward calculations depend only on the linear structure of the blockchain and published finality signatures, rather than block time or consensus mechanism. Reward calculations assume a known constant token supply inflation with nominal platform operation.

**Reward cycle:** Rewards are distributed to validators and delegators at the end of an era for all blocks in that era and several eligible blocks from the previous era.

**Token supply and inflation:** Mainnet launched with ten billion CSPR at the time of genesis. The target annual supply growth rate is 8%.

<!--TODO We might want to update the numbers, ask MAKE (it's also possible Joe has the relevant data)-->
**Annual reward percentage:** Validators on the Casper Mainnet earned between 10% and 20% of their staked CSPR in the first year of the Mainnet operation, with regular participation under expected network conditions. The growth of individual stakes is dependent on the total active stake, as only a fixed number of tokens is created per era.

Please visit the [Staking Guide](./staking.md) for further details on the staking mechanism.
