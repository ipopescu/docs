---
title: Runtime
slug: /runtime
---

# Runtime Economics

The economics of the runtime layer should incentivize efficient allocation of computational resources, primarily CPU time, to users. <!--TODO add a bit more about state pruning here? -->

Currently, [gas](./gas-concepts.md) is allocated according to a first-in, first-out model for transactions. <!--TODO add a bit more about gas here? -->

## Gas allocation {#gas-allocation}

Any finite resource on a publicly accessible computer network must be rate-limited, as, otherwise, overuse of this resource is an attack vector \-- a minimal requirement for platform security. However, rate-limiting, implemented on our platform as a simple block gas limit with several lanes, leaves the platform with the problem of fairly and efficiently allocating the gas. We are researching the optimal structure for spot and futures gas markets, and interested readers should consult the relevant [CEPs](https://github.com/casper-network/ceps). In the meantime, this section outlines some basic features of the platform that would underpin any allocation scheme.

### Consensus before execution & basics of payment {#consensus-before-execution--basics-of-payment}

The Zug and Highway protocols reach consensus on a block _before_ the block is executed, introducing a subtle difference from platforms like Ethereum. In addition, transactions sent to a Casper network can only be selected based on claimed, rather than used, gas.

Additionally, a minimal amount of CSPR must be present in the user account or contract's main purse to cover the payment computation. The community may introduce additional balance checks in the future.

### Costs and limits {#costs-and-limits}

Gas cost is a measure of the relative time used by different primitive operations of the execution engine, which is also assumed to be additive. By additivity, we mean that the time to execute a program is approximately proportional to the sum of gas expended by the opcodes and functions called within the program. Casper assigns gas costs to primitive execution engine opcodes and specific, more complex _host-side_ functions that are callable from within the execution engine context. Gas costs for opcodes and host-side functions are specified in the [chainspec](../glossary/C.md#chainspec) and may vary according to arguments.

<!--TODO remove this line? -->
We expect to refine the current gas cost table to reflect time use more closely, with updates introduced in future upgrades.

### Lanes {#lanes}

The [block gas limit](https://github.com/casper-network/casper-node/blob/b94c4f79ac4ca00e996c418dcc3a98607779a193/resources/production/chainspec.toml#L96-L97) is split into two lanes, one for native transfers and one for general transactions. The number of transfers, which cost a fixed amount of gas, is governed directly by the `block_max_transfer_count` chainspec parameter, set to 2500 on Mainnet. <!--TODO check other lanes cost. -->

## Gas fees {#gas-fees}

See [Gas](./gas-concepts.md). <!--TODO Add a summary and point to new gas concepts page. Mention the new dynamic pricing system for "spot" gas (and "block vacancy"). -->
