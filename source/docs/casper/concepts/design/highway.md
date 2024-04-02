---
title: Highway Consensus
---

# The Highway Consensus Protocol

<!-- The content was moved to consensus.md since most of it was general enough to also apply to Zug. TBD what this page will contain. -->
<!-- TODO confirm that we can use Casper-specific terms in the descriptions, even though Highway is a more general algorithm. For example, a value is a block.>

- nodes communicate by broadcasting units
- A unit contains:
    - citations of other units; at most one citation received per validator node; a unit can be empty at genesis
- nodes are expected to cite the latest unit from every node they have received, including their own latest unit
- if a validator does not follow the process and thus equivocates, their bid gets deactivated; this is a consequence instead of slashing
- when a node equivocates, it can still send units, but it may not be a validator
- the round leader proposes a block with a list of deploys
- Highway works as a "fork choice" where every unit votes on some branch of the chain. As a result, over time, units form a Directed Acyclic Graph (DAG), where units are teh vertices and citations are the edges. (Include a diagram here or a screenshot of B's visualization tool.)
- Highway proceeds in rounds
- A block is finalized if there is a summit among units. A summit is a structure within the graph characterized by a quorum q, which is a percentage of the participating validator weight; at level k in the graph
- For a given fault tolerance threshold t, finality is: (2q-n)(1-2^-k) > t
    - if q is close to 100% of nodes, as k grows, the maximum level of the summit will grow
    - if q is close to n, meaning the whole network participates, a block can be finalized with a high fault tolerance threshold (FTT)
    - the FTT is the weight of the nodes that would have to collude to finalize a conflicting block and revert the transactions in that block; if more than 1/3 of validator weight is faulty and colludes, those nodes will prevent block finalization and stall the network

- Moved from consensus.md, as it pertains mostly to Highway: A block's fault tolerance increases beyond one-third as the protocol continues. If all validators are honest, it approaches 100%.