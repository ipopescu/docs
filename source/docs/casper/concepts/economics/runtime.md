---
title: Runtime
slug: /runtime
---

# Runtime Economics
<!--TODO We will likely need to remove the bit about state pruning, since we will, in fact, now support state pruning. Similarly gas price/"conversion rate" will no longer be fixed to 1 mote per 1 unit of gas ("block vacancy" again)-->
The economics of the runtime layer should incentivize efficient allocation of computational resources, primarily CPU time, to users. Pending state pruning implementation, disk space use is treated as CPU time usage and charged irreversibly per byte written. Currently, [gas](./gas-concepts.md) is allocated according to a first-in, first-out model for deploys, with a fixed price of 1 mote (1/10<sup>9</sup> part of a CSPR token) per 1 unit of gas.

## Gas allocation {#gas-allocation}

Any finite resource on a publicly accessible computer network must be rate-limited, as, otherwise, overuse of this resource is an attack vector \-- a minimal requirement for platform security. However, rate-limiting, implemented on our platform as a simple block gas limit with several lanes, leaves the platform with the problem of fairly and efficiently allocating the gas. We are researching the optimal structure for spot and futures gas markets, and interested readers should consult the relevant [CEPs](https://github.com/casper-network/ceps). In the meantime, this section outlines some basic features of the platform that would underpin any allocation scheme.

### Consensus before execution & basics of payment {#consensus-before-execution--basics-of-payment}

The Zug and Highway protocols reach consensus on a block _before_ the block is executed, introducing a subtle difference from platforms like Ethereum. In addition, deploys sent to a Casper network can only be selected based on claimed, rather than used, gas.

Additionally, a minimal amount of CSPR must be present in the user account's main purse to cover the payment computation. The community may introduce additional balance checks in the future.

### Costs and limits {#costs-and-limits}

Gas cost is a measure of the relative time used by different primitive operations of the execution engine, which is also assumed to be additive. By additivity, we mean that the time to execute a program is approximately proportional to the sum of gas expended by the opcodes and functions called within the program. Casper assigns gas costs to primitive execution engine opcodes and specific, more complex _host-side_ functions that are callable from within the execution engine context. Gas costs for opcodes and host-side functions are specified in the [chainspec](../glossary/C.md#chainspec) and may vary according to arguments.

We expect to refine the current gas cost table to reflect time use more closely, with updates introduced in future upgrades. We also anticipate that storage costs will be calculated separately from computing time with the introduction of state pruning. <!--TODO check this statement for 2.0 Turns out the answer is "no" for now, despite introduction of state pruning. May change in 2.0+-->

### Lanes {#lanes}

The [block gas limit](https://github.com/casper-network/casper-node/blob/b94c4f79ac4ca00e996c418dcc3a98607779a193/resources/production/chainspec.toml#L96-L97) is split into two lanes, one for native transfers and one for general deploys. The number of transfers, which cost a fixed amount of gas, is governed directly by the `block_max_transfer_count` chainspec parameter, set to 2500 when Mainnet launched.

## Gas fees {#gas-fees}

The gas price is currently fixed at 1 mote per 1 gas unit.
<!--TODO See remarks about "block vacancy" above-->

### Fee allocation {#fee-allocation}

<!--TODO This entire section will need a rework-->
All fees from a particular block accrue to its proposer, incentivizing non-empty block production and allowing major dApps to execute deploys for free, provided they operate a validator node and are comfortable with the latency introduced by validator scheduling.
