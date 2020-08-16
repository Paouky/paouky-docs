# Commitments

A commitment scheme is a cryptographic primitive that allows you to commit to a chosen value while keeping it hidden from others, with the ability to reveal the committed value later.

Properties:

* **Hiding** - Nobody but the committer can see or infer the actual value behind the commitment.
* **Binding** - The committer can't change the value after the commitment is published.

ECC can be used to create a commitment. Let’s say we want to commit to the value `8`.

```text
commit (8)  ->  8*G
```

To everybody else, our commitment `8*G` just looks like a random point, and we publish it. Some time later we reveal our value.

```text
reveal 8
```

And now any observer could multiply our stated value 8, by the public point G and verify that their result is equal to the commitment we published ealier.

```text
verify (8, commitment) == 8*G  ? --> True/False
```

However, there’s a major issue. It’s simple for anybody to find out what value we commited to, even if we don’t reveal it; By trying out, or guessing, different values, a commitment that’s equal to ours can be found, thus revealing what value we picked before we chose to do so ourselves.

!!! note "Example"
    Say we're betting on how many goals a team would score by the end of the year. Our guess is 23, and we commit to it by publishing the commitment `23*G`. Problem is, it would be trivial for anybody to uncover our guess simply by trying to commit to 1, 2, 3, 4 etc and checking each result if it's equal to our commitment. In this case, our value will be revealed after only 23 simple steps.

What’s the solution?

## Blinding Factor

The issue is solved by adding a blinding factor `r`, which is a random 256-bit integer used to blind the value so that it can’t be guessed and uncovered.


We could try adding the blinding factor by comitting `(8+r)*G` and then revealing `8` and `r`. But doing so breaks the binding property of the commitment, for reasons we won’t get into here. We require a different method to include `r`.

## Pedersen Commitments

Introducing **G**’s twin, **H**.

**H** is another generator point, distinct from **G** (note how it’s the next letter in the alphabet). Both are nothing-up-my-points, meaning nobody knows `n` such that `G = H`. Using **H** we can blind the value while keeping the commitment binding.

```text
r*G + v*H
```


:   `r` is the blinding factor, and `r*G` is the public key point for `r` (using H as generator point).

    `v` is the value commited, and `v*H` is the public key point for `v` (using G as generator point).




This specific form of commitement is called a *Pedersen Commitment.*

## Homomorphic Commitments

Commitments with homomorphic properties means you can perform calculations on encrypted values without decrypting them first. The result of the computation is a commitment which is identical to the result if the operations had been performed on the unencrypted values.

They allow us to do as follows:

:   commit (x)  &rArr;  C~1~ </br>
	commit (y)  &rArr; C~2~ </br>
	reveal (x + y) </br>
	verify (x + y, C~1~, C~2~) ? &rArr;  True/False

We don’t want to reveal `x` and `y` independently. Instead, we reveal only their *sum*, proving we know the value of each constituent part.

Elliptic curve commitments indeed have these homomorphic properties. We can do the following:

```text
x*G + y*G => (x + y)*G
```

Notice how we add two different commitments and the resut is a commit to the sum of the values we’re hiding. We can then reveal the sum `x + y` to prove that we had knowledge of both `x` and `y` individually, while keeping them a secret.

Similarly, we can add up two Pedersen Commitments. First let's create two of them:

* C~1~ = r~1~G + v~1~H
* c~2~ = r~2~G + v~2~H

The point **Z** (remember a commitment is simply a point on the curve) is the result of addition between points **C~1~** and **C~2~**.

```text
Z = C1 + C2
```

So we can calculate what **Z** is:

```text
Z = r1*G + r2*G + v1*H + v2*H
```

Result:

```text
Z = (r1 + r2)*G + (v1 + v2)*H
```

Hence point **Z** is a pedersen commitment that is the sum of commitments **C~1~** and **C~2~**. This is the foundation of Mimblewimble outputs, as we're about to see.
