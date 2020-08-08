# Privacy

## Overview

Privacy is instrumental to a peer-to-peer electronic cash system. In its essence, cash cannot distinguish between users, it does not reveal the amounts transferred or owned by individuals, and it holds no bias to the history of a specific coin or person.

Grin aims to preserve its user's privacy, while also keeping in mind its core attributes of minimalism and scalability. That said, by cleverly employing Mimblewimble along with several other methods, it is able to achieve a relatively high level of anonymity for its users. This document sets to explore how and to what extent.

First, there are no amounts. A Mimblewimble implementation natively uses Confidential Transaction, meaning all amounts are hidden; They are provably impossible to uncover, yet easily verified. Even before anything else, simply hiding amounts makes any analysis significantly more challenging.

Notably, there are no on-chain addresses either. Transactions hold minimal information, and each output is simply a commitment: a point on the curve. A transaction might look like this:

:   **Transaction**

    | Inputs      | Outputs                              |
    | ----------- | ------------------------------------ |
    | 08c482407fac2.....e335bf2f10d82      | 085cc6944467b.....a3f1d4274b79b |
    |                                      | 097b2588fd494.....494e43580476b |

!!! note ""
    Each transaction also carries rangeproofs and a kernel, but it's mostly irrelevant for this topic.

The above illustrates a normal transaction of 1 input and 2 outputs. The outputs (an input is also a reference to an output) are commitments, a 33 byte blurb to any observer. There's no address to tie an identity to, and it's not clear which output is the change and which belongs to the receiver.

In a Bitcoin-like system, there are multiple ways in which a user might (accidentally or intentionally) link an address to his identity. Consequently it is often trivial for analysis to link many of his addresses. Not only does his own privacy suffer, but also the entire network's privacy diminishes as a result.

A Grin commitment is one unique output. An address may be used to create an unlimited amount of outputs. An interesting analogy could then be used to emphasize the difference:

* A commitment, once on-chain, is like a strictly-one-time-use address. Both of them only ever "contain" a single output.
* A commitment is *not* used to form transactions, unlike an address. A commitment is infact a *result* of a transaction after it was built peer-to-peer. This makes it significantly harder to link an identity to.

Furthermore, a key concept of Mimblewimble transactions is that several of them can be safely merged together, resulting in what looks like a single transaction. When done at the block level, every block essentially becomes one large transaction:

:   **Block**

    | Inputs      | Outputs                              |
    | ----------- | ------------------------------------ |
    | 08c482407fac2.....e335bf2f10d82    | 097b2588fd494.....494e43580476b  |
    | 0857b6b7eb6a2.....a0a283ed35974    | 09f731e071316.....42dae69672dca |
    | 085e205dea687.....8b8aeac7562c6    | 085cc6944467b.....a3f1d4274b79b |
    | 09035d331b17a.....bb76238f605fb    | 094262a95a67a.....2f246f6ce60ce |
    | 0961ee1db49ad.....602489c9c4517    | 09cf2db66b748.....7327297b8e69c |
    |                                    | 09c2751af8fe9.....fc745808238b6 |
    |                                    | 0900015eec3c1.....d52d78fca78de |

The outcome is a non-interactive CoinJoin with hidden amounts. It's called non-interactive since all transactions are aggregated into one without any coordination required between the different parties, as it's made possible at the protocol level. </br>

An observer knows how many transactions are included in the block, since each one carries a kernel, but nothing more. Any further information is impossible to obtain by looking at the chain.

## Transaction Graph


While chain analysis can extract very little (if any) information about users and outputs, it is possible to monitor peer-to-peer network activity and obtain the transactions before they're included in a block and aggregated with others. By setting up sniffing nodes connected to many peers, you can figure out which outputs are being spent by which transaction, allowing you to build a transaction graph. It's unclear at this points what information could be derived from this, as the trail of data stops there.

(**paragraph not worded**)
Grin has many methods that may drastically improve privacy such as payjoin, coinswap etc and the beauty about them is that unlike bitcoin, your coins arent tainted for participating in what is pretty obviously a speicif mixing transaction. a payjoin/coinswap/aggregated tx looks exactly the same, nobody knows you extra privacy precautions to obfuscate the transaction graph



## Dandelion

An important piece of information that commonly leaks is the *IP address* that originally sent the transaction. Normally, a transaction is just broadcasted to all connected peers and spreads quickly on the network, allowing for statistical analysis to deduce where it originated. In a peer-to-peer network, this might be hard as transactions are relayed, but over multiple transactions it becomes trivial.

To tackle this issue, Grin employs Dandelion (originally proposed as a BIP), a protocol designed to hide a transcation's origin IP address. Dandelion has two phases; a `stem` phase and a `fluff` phase. Once a transaction is initially broadcasted, it enters the `stem` phase, in which it it hops between individual peers. At a random point, the transcation enters its `fluff` phase and is spread (fluffed) among the entire network.

This makes it almost impossible to deduce a reliable IP address, and renders statistical analysis impractical.

![dandelion](../assets/images/dandelion.png){ width=675 }

Moreover, Dandelion provides an additional benefit unique to Mimblewimble, as it allows for transactions to be aggregated at a very early stage. If two or more transactions cross paths in their `stem` phase, they are aggregated into a single transaction prior to being fluffed, thus obscuring linkability of inputs and outputs that a sniffing node may have learned. However it remains to be seen how much actual privacy is gained from aggregation before fluffing, as it depends on having many other transactions.

## Afterword

Privacy is complex and information leakage is surprisingly easy. Privacy-preserving systems need to be extremely strong to ensure reasonable amounts of protection. Unfortunately, they often fail in practice simply because they are cumbersome to use, causing people to revert to convenience.

Grin is commited to long term privacy protection and will continue to advance in that direction, while remaining practical and accessible to all, regardless of a person's sophistication or available resources.
