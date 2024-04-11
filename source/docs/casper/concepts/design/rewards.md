---
title: Rewards Design
---

# Network Participation Rewards

<!-- TODO Check existing pages describing rewards under economics. Refactor and update these pages. -->

Validators receive rewards for participating in consensus and thus securing the network. Delegators also receive rewards indirectly by staking with a validator. This page serves as an introduction to how the rewards are calculated and distributed. For more details, refer to the corresponding CEP. <!-- TODO Link here to the future CEP. -->

Like other Proof-of-Stake chains, a Casper network rewards validators for participating in building a linear chain of blocks, each containing ordered state changes and ensuring that the entire ecosystem of validators, builders, and users eventually achieve common knowledge of the chain's history. External, non-validator participants in the ecosystem can thus have a high degree of confidence on the canonical history of the blockchain's state, thus making the blockchain economically useful.

<!-- TODO link to Zug and Highway once PR 1427 merges-->
The network uses a new reward scheme that does not depend on the details of the consensus protocol and is compatible with both Zug and Highway. The current reward scheme has the following properties:

- Rewards are proportional to a validator's weight.
- The reward scheme incentivizes cooperation.
- Rewards are distributed for all blocks.
- Reward calculations depend only on the linear structure of the blockchain.
- Reward calculations assume a known constant token supply inflation with nominal platform operation.

Previously, with Highway, the reward scheme incentivized block production and participation in their finalization. Unfortunately, this scheme was highly coupled with the consensus protocol and unsuitable for adaptation with the Zug consensus. The current scheme calculates rewards after blocks have been finalized, as described below.

## Calculating Rewards

The execution engine calculates rewards for block production and finality signature generation and distribution in each switch block. Finality signatures are produced after a block has been finalized by consensus. Thus, rewards are independent of the consensus algorithm used. The system collects those finality signatures and includes them in future blocks. The rewards scheme allows blocks to cite finality signatures for several past blocks so that validators can agree on which finality signatures have been produced and should be rewarded.

Rewards are divided into these categories:

- **Block rewards**: These rewards are received for each proposed block that is finalized. They incentivize timely participation in building the chain.
- **Finality signature rewards**: These rewards are received for collecting finality signatures for each block and generating a finality signature to sign a block. They incentivize agreement on which blocks are finalized.

In each round, a total reward pool is shared among all participating validators proportionally to their weight. The `round_seigniorage_rate` setting in the chainspec determines the reward pool as a fraction of the total supply. This value is calculated based on the total supply, target inflation rate, and minimum round length.

Each switch block triggers a reward calculation. To account for potential network lag delaying the timely arrival of signatures for finalized blocks, the calculation "looks back" into the previous eras. The number of eras to look back into is specified using the `signature_rewards_max_delay` setting in the chainspec. The rewards formula is additive and calculates rewards over two or more switch blocks.

Blocks carry information on their proposer and the finality signatures collected for several past blocks. Global state contains data on token supply and validator weights as part of the Mint and Auction states. Based on these inputs, the rewards are calculated according to a formula. Rewards are designed to be proportional to weight as long as the impact of platform performance is nominal. <!-- TODO Link to the formula in the CEP or elsewhere -->

Validators are motivated to include finality signatures as quickly as possible. If they do not include a finality signature in a block, the next validator can include it in their block and get the collection fee.

### Chainspec settings for calculating rewards

Each Casper network chainspec contains 4 settings related to calculating rewards:

- `finders_fee`: The split in finality signature rewards between the block creation and participating signers. The block creation reward is usually smaller and goes to the proposer in full.
- `finality_signature_proportion`: The proportion of baseline rewards going to reward finality signatures specifically.
- `signature_rewards_max_delay`: The number of eras to look back into for the reward calculation.
- `round_seigniorage_rate`: Setting that calculates the fraction of the total supply that will constitute the reward pool for every round.

<details>
<summary><b>Expand to see sample values</b></summary>

```json
# The split in finality signature rewards between block producer and participating signers.
finders_fee = [1, 5]
# The proportion of baseline rewards going to reward finality signatures specifically.
finality_signature_proportion = [1, 2]
# Lookback interval indicating which past block we are looking at to reward.
signature_rewards_max_delay = 3
...
# Round seigniorage rate represented as a fraction of the total supply.
#
# Annual issuance: 8%
# Minimum block time: 2^14 milliseconds
# Ticks per year: 31536000000
#
# (1+0.08)^((2^14)/31536000000)-1 is expressed as a fractional number below
# Python:
# from fractions import Fraction
# Fraction((1 + 0.08)**((2**14)/31536000000) - 1).limit_denominator(1000000000)
round_seigniorage_rate = [7, 175070816]
```

</details>


### Rewards distribution summary

The following steps summarize the rewards distribution mechanism for each round.

Each round has a reward pool set in the chainspec under the `round_seigniorage_rate`.

In each round, the reward pool is split into two parts for block creation and finality signatures. The split is configurable using the `finders_fee` setting in the chainspec:
- A percentage is allocated for block creation.
- A percentage is allocated for finality signatures.

The amount allocated for finality signatures is split further into two parts: creating and collecting finality signatures. The split is configurable in the chainspec using the `finality_signature_proportion` setting. 

For each finality signature:
- The creator gets a portion of what was allocated for creating signatures, proportional to its weight.
- The block proposer gets a portion of the collection allocation proportional to the creator's weight.

<p align="center">
<img src={"/image/design/rewards-pot.png"} alt="Pie chart showing how rewards are split" width="300"/>
</p>