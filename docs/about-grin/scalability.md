(**WORK IN PROGRESS**)

A Mimblewimble chain is massively-prunable, which allows it to stay lightweight and cheap to verify. This unique balance of privacy and scalability is not trivial.

Once an output is spent, there's no longer any need to keep it and it's removed from the chain. A 100 byte kernel is left for each transaction, forever proving it's validity. The result is, that in order to verify the entire history starting from the genesis block, only the kernels and the set of unspent outputs are needed. Let's illustrate a trail of transcations:


:   *(switch between tabs)*

    === "Before"

        | Inputs     | Outputs      |            |
        | :--------: | :----------: | :--------: |
        |            | t r a n s a c t i o n |   |
        | Coinbase   | A            | Kernel     |
        |            | t r a n s a c t i o n |   |
        | A          | B            | Kernel     |
        |            | C            |            |
        |            | t r a n s a c t i o n |   |
        | B          | D            | Kernel     |
        | Coinbase   | E            |            |
        |            | F            |            |
        |            | t r a n s a c t i o n |   |
        | D          | G            | Kernel     |
        | E          | H            |            |

    === "Cut-through"

        | Inputs     | Outputs      |            |
        | :--------: | :----------: | :--------: |
        |            | t r a n s a c t i o n |   |
        | Coinbase   | ==A==        | Kernel     |
        |            | t r a n s a c t i o n |   |
        | ==A==      | ==B==        | Kernel     |
        |            | C            |            |
        |            | t r a n s a c t i o n |   |
        | ==B==      | ==D==        | Kernel     |
        | Coinbase   | ==E==        |            |
        |            | F            |            |
        |            | t r a n s a c t i o n |   |
        | ==D==      | G            | Kernel     |
        | ==E==      | H            |            |

    === "After"

        | Inputs     | Outputs      |            |
        | :--------: | :----------: | :--------: |
        |            | t r a n s a c t i o n |   |
        | Coinbase   |             | Kernel     |
        |            | t r a n s a c t i o n |   |
        |            |              | Kernel     |
        |            | C            |            |
        |            | t r a n s a c t i o n |   |
        |            |             | Kernel     |
        | Coinbase   |              |            |
        |            | F            |            |
        |            | t r a n s a c t i o n |   |
        |            | G            | Kernel     |
        |            | H            |            |


An output is considered 'spent' once it's used as an input. As demonstrated above, every single input can dissappear as well as every spent output, leaving only a small kernel behind. What remains is only the set of unspent outputs and the set of kernels proving its validity.

A full verifier needs the following data:

* The chain of headers
* List of unspent outputs
* Kernels

!!! note ""
    An output is a 33 byte commitment accompanied by a 640 byte rangeproof.

.
</br>
.
</br>
.

At the time of this writing, there were 560M Bitcoin transactions (292GB). Somebody who wants to verify the current state must replay each and every transcation. He will check millions of outputs to eventually produce the current set of 66m unspent outputs (3.75GB).

Mimblewimble shrinks the transaction history such that a chain with Bitcoin's history would need 53GB of data to fully verify the current state, and 44GB to store unspent outputs. This difference becomes much more exciting if one remembers that Mimblewimble employs Confidential Transactions, so that each output requires a large rangeproof. If the current Bitcoin chain had hidden amounts, it's size would have been on the order of several terabyes.


---

??? Note "Ramblings and Inspiration (ignore)"
    I read in a medium post, i think by scalar capital (under General in my Articles & Posts file) that grin does not offer an increased tx/s compared to other coins, but that scalability advantages are only to bootstrapping nodes, as this is much quicker, although not a big deal as there are other solutions.
    They guy is stupid because tx/s and full node size are directly related! any chain can double their claimed tx/s but cutting the block time by half, or doubling their blocksize. What's limiting them is not usuaully bandwith but the resources required to run a full node! it can't get too big otherwise it's not decentralized.

    you're able to fully verify the chain and your coins even on a phone or crappy and cheap hardware, and even in third-world countries


    There’s one wrinkle that must be dealt with, and that’s negative values. If it were possible to create a commitment to a negative value, then it would be possible to effectively print money by creating two outputs such as 10 BTC and -10BTC and then later ignoring the -10BTC output. Confidential Transactions deals with this with a technique called a range proof. A range proof is a cryptographic argument that the commitment satisfies a certain range. In CT, this proves that the committed value is positive without revealing anything else about the value. The range proof is actually the largest part of a CT output, but is essential for ensuring the money supply does not undergo significant inflation.


    Building transactions and verifying them in MimbleWimble is trivial computationally and can easily be done on smartphones or raspberry pies

    remember to mention that the chain can actually become smaller
