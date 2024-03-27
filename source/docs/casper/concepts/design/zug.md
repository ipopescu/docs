---
title: Zug Consensus
---

# Simple and Fast Consensus with Zug

The Casper node was designed with a pluggable consensus protocol in mind. So far the only choice was Highway. Casper 2.0.0 will add Zug, [a much simpler consensus protocol](https://arxiv.org/abs/2205.06314).

Of course, all nodes in a network have to run the same protocol to work together, but when starting a new network or upgrading an existing one, either `Highway` or `Zug` can now be selected as the `consensus_protocol` in the chainspec file. The Casper Mainnet will switch to Zug.


## How Zug Works

Unlike Highway, Zug does not use a communication history DAG. The basic idea is as follows:

* In every round, the round leader proposes a new block, `B`.
* Every validator creates and gossips an _echo_ message, with a signature of `B`.
* When a suitable block `B` has received echoes from 67% of the validators:
    * The next round begins! The next leader can propose a child of `B`.
    * Every validator signs and gossips a _vote_ message, voting :white_check_mark:.
* If this does not happen before a timeout, the validators vote :x: instead.
    * If there are :x: votes from 67%, the next round begins, too.
      The next leader can propose a child from an earlier block and skip this round.
* If there are :white_check_mark: votes from 67%, `B` is finalized and gets executed, together with all its ancestors. (Usually, the next round has already started at this point.)

Notice that proposals, votes, and echoes are gossiped, so if one correct node receives a message, all nodes will eventually receive it. An honest validator sends only one echo or vote per round. So, unless 34% of validators double-sign, at most one block per round gets 67% echoes, and no finalized block can ever be skipped, ensuring safety. As long as there are 67% of echoes for a proposal, the next round begins and Zug doesn't get stuck. If there are not, everyone votes :x:, and the next round also begins.


## Some Advantages of Zug

* Apart from the leader, who has to send the proposed block, each validator node broadcasts only two tiny messages in each round, making the communication overhead very small.
* Unlike in Highway and Practical Byzantine Fault Tolerance (PBFT), and similar to pipelined protocols like HotStuff, only one round of messages (the echoes) is needed for the next leader to propose a block, reducing the delay between a block and its child.
* But _unlike_ HotStuff, Zug can finalize a block without waiting for its child or grandchild. And, unlike Highway, it does so without waiting for any timeout. Even if a network is configured to produce only one block per minute, every block gets finalized within seconds, as fast as the network connections allow.
* Zug's technical description is more flexible than Highway's, giving us a family of related, correct implementations from which to choose.


## Comparison with Highway

Highway sends larger messages and is a bit slower, but allows for more fine-grained [block rewards (see below)](#Block-Rewards).

Highway also allows different clients to follow the protocol using different fault tolerance thresholds, with different tradeoffs between security and latency. However, _if_ enough validators are online, Zug has lower latency than Highway with any threshold.

Finally, Highway is much more complicated than Zug: it takes more than twice as many lines of code to implement it. So Zug will make it easier for third parties to create compatible node software that works together with the Casper node.

Highway's proof of correctness has proved to be more difficult for people to verify than Zug's.


### Block Rewards

Using a DAG makes fine-grained information about the validators' behavior available on-chain in Highway, so block rewards can be tuned to incentivize full participation in the consensus protocol. However, this does not apply at the end of each era. Any message sent after the era's last block was proposed cannot be taken into account, even though these messages are still needed to finalize that block. And this granularity comes at a cost: Highway messages are relatively big.

The current system does not reward finality signatures, which are arguably the most important outcome. The signatures are the user-visible proof, signed by the validators, that an executed block is correct.

In Zug, messages are much smaller, so a smaller incentive is needed to send them.

Casper 2.0.0 will distribute a configurable fraction of the seigniorage as a reward for finality signatures and the rest as a simple reward for each block, both proportionally to the validators' stakes.

This new reward system is simpler, fairer, predictable, and transparent. It will give equal weight to all blocks (including at the end of an era), but it will not take into account every single consensus message.


## Read the Paper

Here, we describe Zug, an implementation of the ideas from our paper [From Weakly-terminating Binary Agreement and Reliable Broadcast to Atomic Broadcast](https://arxiv.org/abs/2205.06314). The paper, however, contains a much more general algorithm parameterized by the two subprotocols named in the title: Reliable Broadcast and Weakly-terminating Binary Agreement. In our specialization of this algorithm made for the Casper blockchain, the _echo_ messages are used by our Reliable Broadcast implementation, and the _vote_ messages are used by our Weakly-terminating Binary Agreement implementation.
