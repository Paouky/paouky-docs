
---
description: Mimblewimble
---

**Preface**

The following three-part series is intended for those who wish to understand what goes on inside Mimblewimble and Grin. We try to make this objective more accessible by going through the cryptographic primitives required to know first, without diving into proofs and specifics. Once those are in order, we connect everything explain how Mimblewimble works.

For the original introduction written by Igno, read [here](https://github.com/mimblewimble/grin/blob/master/doc/intro.md).

---

# Elliptic Curve Cryptography

Mimblewimble relies entirely on Elliptic-curve cryptography (ECC), an approach to public-key cryptography. Put simply, given an algebraic curve of the form `y^2 = x^3 + ax + b`, pairs of private and public keys can be derived. Picking a private key and computing its correspnding public key is trivial, but the reverse operation `public key -> private key` is called the *discrete logarithm problem*, and is considered to be computationally infeasible.

Let's review the basics.

## Operations

These are the relevant mathematical operations we can do on Elliptic-curve points.

* Addition - Given two points, we can add them to one another (or subtract) and the result would be a new point on the curve.
* Multlipication - Given a point, we can multiply it any number of times.

#### Addition

Given three aligned points **P**, **Q** and **R**, their sum is always 0. We treat this as an inherent property of elliptic curves.

```text
P + Q + R = 0
```
We can then write it as:

```text
P + Q = -R
```

So that adding the two points P and Q results in -R, the inverse of R.

If we draw a line passing through **P** and **Q**, this line will cross a third point on the curve, **R** (so that **P**, **Q** and **R** are aligned). If we take the inverse of this point, which is simply the one symmetric to it about the x-axis, we have found the result of adding two curve points, **P + Q**. Let’s illustrate:

[P + Q = R](../assets/images/ecc-1.png)

#### Multlipication

We can’t multiply a point by another point, but we can multiply a point by a number (scalar). Multiplying point **P** by scalar `k` would simply require adding point **P** onto it self ``k` times. This operation is easily demonstrated by assigning `k=2` so that `k*P = P+P`. To illustrate how it would look like on the curve, we draw a tangent line. You can imagine that the line intersects three points, whereas two of them are **P**, such that:

```text
P + P = -R
```

[P + Q = -R](../assets/images/ecc-2.png)

!!! note ""
    To calculate `8*P` for e.g. wouldn’t take 8 operations, but only 3; you can find 2P, then add it onto itself, and then add 4P onto itself, for the final result of 8P.

## Key Pairs

An ECC system defines a public constant curve point called the generator point, **G** (known to everybody). The generator point is used to compute any public key. A key pair consists of:

* Private key **k** – A randomly chosen 256-bit integer (scalar).
* Public key **P** – An Elliptic-curve point derived by multiplying generator point **G** by the private key.

And more clearly, a public key (of private key `k`) is as follows:

```text
P = k*G
```

This is easy to compute.

But, if everybody knows points **P** and **G**, can they find out what `k` is? The answer is no; The difficulty of getting from one point to another is precisely the definition of the Elliptic curve discrete logarithm problem.

!!! note "Secp256k1"
    The specific Elliptic curve that Grin employs is rust-secp256k1 (y^2^ = x^3^ + 7) using Schnorr signature scheme.
