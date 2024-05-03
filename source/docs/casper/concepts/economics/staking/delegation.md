---
slug: /concepts/economics/delegation
---

# Delegation Details

import useBaseUrl from '@docusaurus/useBaseUrl';

This section provides a detailed explanation of the delegation cost mechanism, how the gas cost relates to delegations, where to find the details, etc. Please note that the cost amounts are likely to change over time, and you may have to check the latest release details to get the most up-to-date and accurate details.

## Delegation Cost

The delegation cost is defined in the chainspec.toml file of a Casper network. In this [example chainspec](https://github.com/casper-network/casper-node/blob/release-2.0.0-rc1/resources/production/chainspec.toml), the delegation is set to cost 2.5 CSPR. However, `when you perform the delegation, you see that it costs a little more` than what is specified in the chainspec. Let's discuss why this happens.

When you delegate, the system automatically charges some gas to set up related data in the global state of the network to track your delegation. This cost is added to the delegation cost defined in the chainspec file.

For example, the chainspec file in release 2.0.0 contains the following information. Notice the delegation cost specified with `delegate`.

```rust
[system_costs.auction_costs]
get_era_validators = 10_000
read_seigniorage_recipients = 10_000
add_bid = 2_500_000_000
withdraw_bid = 2_500_000_000
delegate = 2_500_000_000
undelegate = 2_500_000_000
run_auction = 10_000
slash = 10_000
distribute = 10_000
withdraw_delegator_reward = 10_000
withdraw_validator_reward = 10_000
read_era_id = 10_000
activate_bid = 10_000
redelegate = 2_500_000_000
```

Delegation fees may change over time, so it is essential to stay current. To do so, select the latest release in [Github](https://github.com/casper-network/casper-node) and navigate to the `resources/production/chainspec.toml` file.

For further questions, please join the [Discord channel](https://discord.com/invite/casperblockchain).

## First-time Delegation

If you perform a delegation for the first time, the system charges some motes to create a purse to hold the delegated tokens.

**Example:** The system can charge 0.5 CSPR in addition to the base delegation fee of 2.5 CSPR, resulting in a delegation cost of 3 CSPR on [Mainnet](https://cspr.live/).

When you set up a delegation transaction, it is essential to have enough funds in your account's main purse. Otherwise, the transaction will fail, and you will lose the transfer cost. For example, if you have 200 CSPR in your purse, delegate at most 197 CSPR and leave at least 3 CSPR for the delegation cost. Another option is to delegate 195 CSPR and leave some additional buffer.

As a result, when performing a [delegation using the command line](../../../developers/cli/delegate.md), we recommend you specify a little more than the base transaction payment to ensure that the transaction will go through without failure.

<p align="center"><img src={useBaseUrl("/image/economic-delegationDetails.png")}  alt="details" width="400" /> </p>

<p align="center">

**Figure 2** : On Testnet or Mainnet, the transaction fee for a delegation is a little bit higher than 2.5 CSPR </p>

:::note

Transaction costs depend on each Casper network and the cost tables defined in the chainspec. Most of these examples are from the Casper Mainnet or Testnet.

:::

Lastly, we recommend that you try out delegations on the [Casper Testnet](https://testnet.cspr.live/) before making transactions on the [Casper Mainnet](https://cspr.live/). This will help you understand the costs involved in delegating tokens.
