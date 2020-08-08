Pailliar cryptosystem:

The scheme is a an additive homomorphic cryptosystem; this means that, given only the public key and the encryption of m 1 and m 2, one can compute the encryption of m 1 + m 2.

in our case, when two outputs are encypted (commits), one can still compute the encryption of the sum of both outputs, and verify that it's a multlipiaction of the G generator point.

from original whitepaper:
We can imagine now each block as one large transaction. To validate it, we add all the
output commitments together, then subtracts all input commitments, k*G values, and all
explicit input amounts times H. We find that we could combine transactions from two
blocks, as we combined transactions to form a single block, and the result is again
a valid transaction. Except now, some output commitments have an input commitment exactly
equal to it, where the first block's output was spent in the second block. We could
remove both commitments and still have a valid transaction. In fact, there is not even
need to check the rangeproof of the deleted output.

The extension of this idea all the way from the genesis block to the latest block, we
see that EVERY nonexplicit input is deleted along with its referenced output. What
remains are only the unspent outputs, explicit input amounts and every k*G value.
And this whole mess can be validated as if it were one transaction: add all unspent
commitments output, subtract the values k*G, validate explicit input amounts (if there
is anything to validate) then subtract them times H. If the sum is 0, the entire
chain is good.



My personal preference for MimbleWimble comes from its extreme simplicity (especially compared to zk-snarks) and great scalability. It's also less opaque than zcash, you can still reason about inputs and outputs while all identifiable information about them is obscured.


Confidence in certain cryptography is gained by having it last a long time without being broken. Mimblewimble only relies on elliptic curves; simple and very well vetted cryptographic constructs and assumptions.
