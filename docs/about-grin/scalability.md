I read in a medium post, i think by scalar capital (under General in my Articles & Posts file) that grin does not offer an increased tx/s compared to other coins, but that scalability advantages are only to bootstrapping nodes, as this is much quicker, although not a big deal as there are other solutions.
They guy is stupid because tx/s and full node size are directly related! any chain can double their claimed tx/s but cutting the block time by half, or doubling their blocksize. What's limiting them is not usuaully bandwith but the resources required to run a full node! it can't get too big otherwise it's not decentralized.


There’s one wrinkle that must be dealt with, and that’s negative values. If it were possible to create a commitment to a negative value, then it would be possible to effectively print money by creating two outputs such as 10 BTC and -10BTC and then later ignoring the -10BTC output. Confidential Transactions deals with this with a technique called a range proof. A range proof is a cryptographic argument that the commitment satisfies a certain range. In CT, this proves that the committed value is positive without revealing anything else about the value. The range proof is actually the largest part of a CT output, but is essential for ensuring the money supply does not undergo significant inflation.


Building transactions and verifying them in MimbleWimble is trivial computationally and can easily be done on smartphones or raspberry pies
