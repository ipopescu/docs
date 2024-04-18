---
slug: /concepts/economics/concepts
---

# Staking Concepts

The Casper Mainnet is a Proof-of-Stake blockchain that allows validators to stake the Casper native token CSPR on the network. Validators receive CSPR as an incentive for continuing to maintain and secure the network. CSPR rewards are distributed as blocks are validated into existence and organized into eras.

**Consensus mechanism:** The Casper Mainnet and Testnet use a Proof-of-Stake consensus mechanism called [Zug](../../design/zug.md). Another Casper network can choose between Zug and [Highway](../../design/highway.md) using the network's chainspec.

<!--TODO update the number of validators with 2.0 -->
**Number of validators:** The Casper Mainnet supports up to 100 validators on the network. This number is chosen to strike a balance between performance and decentralization. This platform parameter can be increased through upgrades as development continues and performance improves. In addition, validators can stake on the Casper Mainnet through a process of permission-less bonding by participating in an auction for the validator slot.

**Permission-less bonding:** For validators to begin staking and earning rewards, they must win a staking auction by competing with current and prospective validators to supply one of the forthcoming top stakes for a given era. This process is permission-less, meaning validators can join and leave the auction without restrictions, except for a waiting period to unlock staked tokens.

**Unbonding:** To detach from the Casper Mainnet, it takes seven eras for both validators and delegators. Neither validators nor delegators receive rewards during the seven eras required for unbonding, as they are not actively contributing to the network's security during that time.

<!--TODO update block times with 2.0-->
**Eras and block times:** An era on Casper is roughly 2 hours long. Casper's consensus protocol allows validators to propose blocks as quickly as network conditions allow, subject to a platform-wide limit that may be adjusted with upgrades. We anticipate block times to last between sixteen seconds and eight minutes.

**Block rewards:** Block time is orthogonal to [rewards](../../design/rewards.md), so there is no precise reward per block. Instead, the number of rewards is split proportionally among stakes and reduced for failure to participate in the protocol promptly.

**Reward cycle:** Rewards are distributed to validators and delegators once per era.

**Token supply and inflation:** Mainnet launched with ten billion CSPR at the time of genesis. The target annual supply growth rate is 8%.

**Annual reward percentage:** Validators on the Casper Mainnet earned between 10% and 20% of their staked CSPR in the first year of the Mainnet operation, with regular participation under expected network conditions. The growth of individual stakes is dependent on the total active stake, as only a fixed number of tokens is created per era.

Please visit the [Staking Guide](./staking.md) for further details on the staking mechanism.
