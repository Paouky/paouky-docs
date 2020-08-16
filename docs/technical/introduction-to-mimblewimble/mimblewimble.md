# Mimblewimble

Earlier we demonstrated how a public key obtained from the *addition of two private keys* `r` and `v`, resulting in `(v+r)*G`, is identical to the *addition of the public keys* of each individual private key, `r*G + v*G`. Mimblewimble and Grin heavily rely on this principle.

## Transactions

Every transaction has to prove two basic things:


* **Zero sum** - The sum of outputs minus inputs should always equal zero, proving that a transaction did not create new coins, without revealing the actual amounts.

* **Possession of private keys** - ownership of outputs is guaranteed by the possession of ECC private keys. However, the proof that an entity owns those private keys is not achieved by directly signing the transaction, as with most other cryptocurrencies.

Going next, we'll examine how those two fundemental properties are achieved.

### Amounts

Building upon the ECC princple desrcibed above, we can obscure the values in a transaction.

If `v` is the amount value of an input or output and **H** is a generator point on the elliptic curve, we can simply embed `v*H` instead of `v` in a transaction. This works because using the ECC operations, we can validate that the sum of values in outputs, equals to the sum of values in inputs. If we subtract those sumb, the result would be 0.

:   *transaction*

    === "Hidden"

        | Inputs     | Outputs      |
        | :--------: | :----------: |
        | v~1~H      | v~2~H        |
        |            | v~3~H        |

    === "Transparent"

        | Inputs     | Outputs      |
        | :--------: | :----------: |
        | v~1~       | v~2~         |
        |            | v~3~         |

```text
v3*H + v2*H - v3*H = (v3 + v2 - v1)*H = 0*H
```

!!! note ""
    Recall that an input is just a reference to a past output being spent, so each input is practically considered an output.

Verifying this property on every transaction allows the protocol to confirm that a transaction doesn't create money out of thin air, without knowing what the actual values are.

However, we encounter a familiar issue; There are a finite number of usable values (amounts) and one could try every single one of them to guess the value of the transaction.

### Blinding Factor

We introduce a blinding factor `r`, also used as a private key (explained later). Along with it we use a second generator point, **G**, on the same elliptic curve.

Then, an output is expressed as:

```text
r*G + v*H
```

* `r` is both a blinding factor and a private key, and its public key is r*G (using G as a generator point).
* `v` is the value of an output, and its public key is v*H (using H as a generator point).

The form of a Mimblewimble output is a Pedersen Commitment. Neither `v` nor `r` can be deduced, but the commiter can prove knowledge of them.



We include `r` to obtain the following transaction.

:   *transaction*

    | Inputs             | Outputs           |
    | :----------------: | :---------------: |
    | r~1~G + v~1~H      | r~2~G + v~2~H     |
    |                    | r~3~G + v~3~H     |

```text
(r2*G + v2*H) + (r3*G + v3*H) - (r1*G + v1*H) = (r2 + r3 - r1)*G + 0*H
```

### Ownership

In the previous section we introduced a blinding factor to obscure the transaction's values. Another insight of Mimblewimble is that this blinding factor can be leveraged to prove ownership of the value, serving also as a private key to an output.

Let's illustrate this using an example:

*Alice* sends 3 coins to *Bob*, and to obscure the amount, Bob chooses 28 as his blinding factor (in practice it's an extremely large number). The following output appears on the blockchain and is only spendable by Bob.

```text
X = 28*G + 3*H
```
**X** is a commitment visible by everyone. The value `3` is only known to Bob
and Alice, while `28` is only known to Bob.

To transfer those 3 coins again, the protocol requires `28` to be known. Let's say Bob wants to send the 3 coins to *Carol*. He needs to build a simple transaction where `Xi` is his input, and `Y` is Carol's new output. For the transcation to be valid, proving no new coins were created, subtracting the input from the output should result in 0.

```text
Y - Xi = 0*G + 0*H = 0
```

If we want this transaction to sum to 0, it would look like this:

:   | Inputs               | Outputs             |
    | :------------------: | :-----------------: |
    | 28&#42;G + 3&#42;H   | 28&#42;G + 3&#42;H  |

!!! note ""
    Remember that a blinding factor also acts as a private key.

But wait. We just introduced a major problem. Bob knows Carol's new blinding factor, so he can spend her output and steal it.

To solve this, Carol chooses her own blinding factor, `114`. The transaction on the blockchain would then look like this:

:   *transaction*

    | Inputs               | Outputs             |
    | :------------------: | :-----------------: |
    | 28&#42;G + 3&#42;H   | 114&#42;G + 3&#42;H |


```text
Y - Xi = (114*G + 3*H) - (28*G + 3*H) = 86*G + 0*H
```

Now the transaction no longer sums to zero and we have an **excess value** `86`. The excess value of a transaction is the sum of all outputs blinding factors, minus the sum of all inputs blinding factors, r~o~ - r~i~. In this case, simply `114-28 = 86`.

Then how does the protocol verify that values equal to 0? The transaction is only legitimate if `Y - Xi` is a valid public key for generator point G; which is the case **only if** `Y - Xi = ... 0*H`. In other words, if the values don't sum to 0, the result is recognized as an invalid public key for G.

This can be verified by requiring the transactors to build a (schnorr) signature together with the excess value `86`, which ensures that:

* The transacting parties can collectively produce the excess value (since it's the private key of their joint signature).
* The sum of the outputs minus the inputs is 0, because only a valid public key for G will check against the signature.

This signature, along with a commitment to the excess value (86*G), is called a transcation kernel.

#### Change

Let's say Bob wants to send only 2 of his coins to Carol. To do this he would send the remaining 1 coin back to back to himself, by creating an output he controls. Bob generates a private key `13` as a blinding factor for his change output. Carol uses the same private key as before.

:   *transaction with change*

    | Inputs               | Outputs             |
    | :------------------: | :-----------------: |
    | 28&#42;G + 3&#42;H   | 114&#42;G + 2&#42;H |
    |                      | 13&#42;G + 1&#42;H  |


```text
(114*G + 2*H) + (13*G + 1*H) - (28*G + 3*H) = 99*G + 0*H
```

The signature is again built with the excess value, `99` in this example.


### Rangeproofs

Previously, we assumed output values to always be positive. However, by introducing negative amounts, a transcation could be balanced out, for example, with an input of `2` and outputs of `5` and `-3`. This can't be detected as the values would still equal to zero.

A rangeproof is therefore attached to every output and proves that its value isn't negative and that its size is restricted so it doesn't overflow. We won't elaborate on this here.

## Putting it all together

A Mimblewimble transaction includes the following:

:   *full transaction*

    | Inputs               | Outputs             |
    | :------------------: | :-----------------: |
    | 28&#42;G + 3&#42;H </br> Rangeproof | 114&#42;G + 2&#42;H </br> Rangeproof                                                          |
    |                | 13&#42;G + 1&#42;H </br> Rangeproof              |
    |                |                                                  |
    |   **Kernel**:  |    TX fee </br> Signature </br>  Kernel excess   |

* Set of inputs, that reference and spend a set of previous outputs.
* Set of new outputs that each includes:
    1. Value and a blinding factor (a new private key), both multiplied on a curve and summed up to r*G + v*H.
    2. Rangeproof that, among other things, shows that `v` is non-negative.
* Kernel consisting of:
    1. Transaction fee in plain text.
    2. Transaction signature signed by the excess value excess value (and verifies wit hthe kernel excess).
    3. Kernel excess, which is the public key corresponding to the excess value (computed by the `sum of outputs + fee - sum of inputs`)


## Blocks and Chain

We explained above how Mimblewimble transactions can provide strong anonymity guarantees while maintaining the properties required for a valid blockchain, i.e., a transaction does not create money and proof of ownership is established through private keys.

The Mimblewimble block format builds on this by introducing one additional concept: cut-through. With this addition, a Mimblewimble chain gains:

* Extremely good scalability, as the great majority of transaction data can be eliminated over time, without compromising security.
* Further anonymity by mixing and removing transaction data.

### Transaction Aggregation

While the kernel excess of a transaction can be computed by anyone, there is a major benefit in including it in every transactions's kernel, as it allows for aggregation within blocks.

The following is true for any valid transcation (ignoring fees).

```text
transaction:
sum(outputs) - sum(inputs) = kernel_excess
```

The same holds true for blocks themselves once we realize a block is simply an extanded set of inputs, outputs and transaction kernels. We can sum the outputs, subtract the inputs from it, and the result would be a commitment equal to the sum of the kernel excesses.

```text
block:
sum(outputs) - sum(inputs) = sum(kernel_excess)
```

Simplifying slightly (ignoring transaction fees), we can see how entire Mimblewimble blocks can be treated exactly as single transactions.

Similarly, transactions could be aggregated before block construction and thus enter the mempool at an already aggregated state. The Dandelion stem phase does so automatically when possible, and it could also be done manually and potentially through different aggregation services.

### Kernel Offsets

There is a subtle problem with Mimblewimble blocks and transactions as described above, that needs to be addressed. Given a set of inputs, outputs and transaction kernels, a subset of these will combine to reconstruct a valid transaction.

Consider the two following transcations:

```text linenums="1"
(in1, in2) -> (out1)        |  (kern1)
     (in3) -> (out2, out3)  |  (kern2)
```

We can aggregate them into a block (or simply another transcation):

:   *aggregated block/transaction*

    | Inputs               | Outputs             | Kernels          |
    | :------------------: | :-----------------: | :-------------:  |
    | in1                  | out1                | kern1            |
    | in2                  | out2                | kern2            |
    | in3                  | out3                |                  |


Initially it seems that the two original transactions are indistinguishable from one another within the new transaction. However, it is trivial to try all possible combinations of inputs/outputs, and find the way in which they succesfully sum to one of the kernel excesses, such that in our example:

```text
(in1, in2) - (out1) = (excess_kern1)
(in3) - (out2 + out3) = (excess_kern2)
```

We're able to easily uncover the original form of both transactions. </br>
Then how do we solve this?

!!! note ""
    Remember that the kernel excess `r*G` is simply the public key of the excess value `r`.

To address this issue we introduce a *kernel offset* and redefine a transaction's kernel excess from r*G to `(r-kernel_offset)*G`. The kernel offset is thus a blinding factor that needs to be added to the excess value to ensure the sum of the commitments is valid:

```text
sum(outputs) - sum(inputs) + (kernel_offset)*G = (r + kernel_offset)*G
```

If the sum of a transaction's inputs and outputs is the commitment `r*G + 0*H`, the transactors generate offset `a` and the transaction is signed with `(r-a)` instead of `r`. But, `r*G` could still be computed to verify the validity of the transaction because given the new form of kernel excess `(r-a)*G` and the offset `a`.

The verifier computes a*G and calculates:

```text
`kernel_excess + kernel_offset*G = sum(blinding_factors)
```

Translated to:

```text
(r-a)*G + a*G = r*G
```

Given `r*G` the transaction can then be validated.

During aggregation, all kernel offsets are summed to generate a single aggregate kernel offset to cover the whole block (or aggregated transaction). The kernel offset for any individual transaction is then unrecoverable and the subset problem is solved.

### Cut-through

Blocks let miners assemble multiple transactions into a single set that's added to the chain. Inputs reference outputs they spend, which are shown as `in(output_spent)` in the following example.

:   *non-cut-through block*

    | Inputs               | Outputs             |
    | :------------------: | :-----------------: |
    | in1(prev_out)        | out1                |
    |                      | out2                |
    | in2(prev_out)        | out3                |
    | in3(out2)            |                     |
    | in4(out3)            | out4                |
    |                      | out5                |

We notice the two following properties:

* Within this block, some outputs are immediately spent by following inputs (e.g. in3 spends out2).
* The structure of each transaction does not actually matter. Since all transaction values individually sum to zero, then the collective sum of all inputs and outputs must be zero.

Similarly to a transaction, all that needs to be checked in a block is that ownership has been proven (which comes from the transaction kernels) and that the whole block did not create any coins, other than what's allowed as the mining reward. Therefore, matching inputs and outputs can be eliminated, as their contribution to the overall sum cancels out. Which leads to the following, much more compact block:

:   *cut-through block*

    | Inputs               | Outputs             |
    | :------------------: | :-----------------: |
    | in1(prev_out)        | out1                |
    | in2(prev_out         | out4                |
    |                      | out5                |

All transaction structure has been eliminated and the order of inputs and outputs does not matter anymore, while the sum of all inputs and outputs values is still guaranteed to be zero.

A block is simply built from:

* Block header.
* Set of inputs remaining after cut-through.
* Set of outputs remaining after cut-through.
* A single kernel offset (sum of all kernel offsets) to cover the full block.
* The transaction kernels containing, for each transaction:
    1. The public key ``(r-a)*G`, which is the (modified) kernel excess.
    2. The signatures generated using the (modified) excess value `(r-a)` as the signing private key.
    3. Mining fee.

The block contents satisfy:

```text
sum(outputs) - sum(inputs) = sum(kernel_excess) + sum(kernel_offset)*G
                           = (r-a)*G + a*G
```

And it all still validates.

### Cut-through Everything

Going back to the previous example block, outputs `prev_out1, prev_out2`, which were spent by `in01, in02` respectively, must have appeared previously in the blockchain. After the addition of this new block, those past outputs as well as `in01, in02` can also be removed from the blockchain as they now are intermediate transactions.

All that's needed to remain is the set of currently unspent outputs `out01, out02, out05`. We conclude that the chain state (excluding headers) at any point in time can be summarized by just these pieces of information:


1. The total amount of coins created by mining in the chain.
2. The complete set of unspent outputs.
3. The transactions kernels for each transaction.

Both the set of unspent outputs (UTXO) and transaction kernels are extremely compact. This has important consequences; The blockchain a node needs to maintain is very small, as well as the amount of information that needs to be transferred when a new node joins the network.

## Conclusion

We covered the basic principles that underlie Mimblewimble. By using addition of elliptic curve points, we're able to build transactions that are completely opaque but can still be properly validated. And by generalizing those properties to blocks, we can eliminate a large amount of blockchain data, allowing for great scaling and fast sync of new peers.
