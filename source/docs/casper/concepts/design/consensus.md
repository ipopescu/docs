---
title: Consensus in Casper
---

# Consensus in a Casper Network

The Casper blockchain's decentralized nature requires a method through which disparate entities can agree on one immutable truth. In a Casper network, the immutable truth is the chain of finalized blocks, and the disparate entities are the validator nodes, which must determine the validity of transactions, resolve conflicts, and finalize the blocks in the chain. The consensus protocol is a mechanism through which the validators agree on each finalized block. 

## Safety, Liveness, and Byzantine Fault Tolerance

In a Casper network, validator nodes receive different inputs via Deploys from connecting clients. Given the consensus mechanism and rules, all honest nodes will output the same value, which is a finalized block in Casper. The [Deploy Lifecycle](../deploy-and-deploy-lifecycle.md) describes what happens after blocks are proposed and finalized. Each finalized block will contain the set of Deploys, which the network will eventually execute. The property described here, where all honest nodes agree on a final value, is called **safety**.

The consensus protocol ensures that honest validators agree on finalized blocks in a finite time, allowing the network to continue producing blocks indefinitely. This property of the protocol is called **liveness**.

The validators will agree on finalized blocks even if some nodes are faulty. This property makes the consensus protocol tolerant to a **Byzantine fault** and thus secure against malicious activity.

To summarize, the consensus mechanism will determine how a blockchain meets the following requirements:

* **Safety**: All honest nodes eventually agree on the final value, which in a Casper network is a finalized block. The consensus mechanism is set up so that no two honest validators will report two different blocks at the same height of the blockchain.

* **Liveness**: The network runs indefinitely and adds new blocks to the chain.

* **Byzantine Fault Tolerance (BFT)**: All honest nodes eventually agree on the final value, even if some are faulty.

## Casper Consensus Protocols

<!-- TODO is the switch "hot", or is there a protocol to stop the network, switch the config, then restart the network? If it's "cold", has the process been documented somehwere? -->

Each Casper network has the special capability to choose and configure its consensus protocol using the network's chainspec. The protocols available are called [Zug](./zug.md) and [Highway](./highway.md). Since the launch of the Casper Mainnet, the [Highway](https://arxiv.org/pdf/2101.02159.pdf) protocol was in use until the Condor (2.0) release. Over the course of three years, the Zug consensus protocol has been developed to simplify and speed up the consensus process, without compromising safety. The whitepaper written by A. Fackler, S. Schlesinger, and M. Doty entitled [From Weakly-terminating Binary Agreement and Reliable Broadcast to Atomic Broadcast]((https://arxiv.org/abs/2205.06314)) proves that Zug, the new protocol is safe, live, and resilient.

## Consensus in the Casper Mainnet

The Casper Mainnet is a [Proof-of-Stake](../glossary/P.md#proof-of-stake) network in which the on-chain auction contract determines validators participating in consensus. The protocol uses a decentralized network of [nodes](../glossary/N.md#node), which participate in the consensus process by staking CSPR tokens. These active nodes are known as [validators](../glossary/V.md#validator). The top 100 bidders are selected through the auction contract every era to act as validators in the era after the next (current era + 2). Nodes with a greater stake in the network's success have a greater weight in reaching consensus.

The Mainnet will continue to function so long as the amount of faulty or dishonest nodes does not exceed one-third of the total number of nodes in the network. Nodes that are not faulty are *honest* nodes. In most cases, the network can assume that more than two-thirds of all nodes will actively collaborate to achieve consensus. Therefore, stronger-than-average finality guarantees occur during periods when all nodes are acting honestly. A block's fault tolerance increases beyond one-third as the protocol continues. If all validators are honest, it approaches 100%.

:::note

The Zug or Highway consensus protocols do not necessitate a Proof-of-Stake method of choosing validators and could theoretically be used alongside a private network with a different model.

:::

### Dynamic Round Length

Within the Zug or Highway protocols, the length of a round is determined dynamically to ensure a suitable amount of time for nodes to gossip all messages through several round trips with honest validators. This ensures that the system maintains liveness by properly gossiping all messages while adding blocks to the chain in a timely manner.

### Eras

The concept of *eras* allows consensus to reduce the overall operational storage requirements of participating nodes while also rotating validators. On Mainnet, a new instance of Zug runs every two hours or approximately 220 blocks, depending on current network metrics. This allows for two benefits:

* **Data Reduction** - Older "metadata" used in finalizing certain blocks is no longer useful and can be removed without compromising the immutability of the data stored on the blockchain.

* **Banning Equivocators** - Dishonest nodes caught equivocating in a previous era are banned from participating in new eras. This allows honest nodes to begin a new era in the *relaxed mode*, no longer needing to send endorsements due to past equivocations.

* **Rotating Validators** - Bonded nodes bid on validator spots each era, with the top 100 highest bidders becoming validators for the era after next (`current era`+ 2).

In any given era, node operators will bid to become validators that will participate in the consensus mechanism for the era after next (`current era`+ 2). Each time slot within the era will also determine a lead validator. The lead validator proposes new blocks to be added to the chain, which are then gossiped among the network's nodes. These messages show an implicit preference for the lead validator's block due to the GHOST (Greedy Heaviest Observed Sub-Tree) rule. Once this process reaches critical mass, with a sufficient interconnected pattern of messages, it becomes impossible to switch to another block. The selected block is then considered finalized and added to the chain.

The final block of an era is a *switch block* and forms the initial state of the next era. A new Zug instance begins with the new era, using information contained within the *switch block* and a new potential set of validators. More details on the auction process to determine an era's validators can be found within the [Consensus Economics](../economics/consensus.md) page.

### Finality

Finality occurs when the network can be sure that a block will not be altered, reversed, or canceled after addition to the chain. This occurs via consensus, and as all transactions happen within a block, it allows for confirmation that a transaction cannot be changed. After finality, it would require greater than 1/3 of all validators to double-sign to cause a disparity between nodes. In this event, the network would shut down and require a manual restart.

On a Casper network, a transaction finalizes alongside the finalizing of the block in which it is included. Validators that equivocate risk eviction, in which the network removes them from the validator set. Therefore, honest nodes receive rewards for their participation, while equivocating nodes risk loss of revenue for acting maliciously.

Zug and Highways's criterion for detecting finality is the presence of a pattern of messages called a `Summit`. It is an improvement over previous CBC Casper finality criteria, which were more difficult to attain and computationally more expensive to detect. Summits preserve the advantage of tunable fault tolerance while being detected in polynomial time.

## Important Links

- [Zug Consensus](./zug.md) - An overview of the Zug consensus used in Mainnet and Testnet
- [Highway Consensus](./highway.md) - Brief overview of the Highway consensus available as an alternative to Zug