# Welcome to the Grin Documentation

**(WORK IN PROGRESS, NOT A REAL DOCUMENT YET)**

Green field implementation that strives to be as clear and simple as possible, making it easier to audit and maintain in the future.


Simplicity, Privacy, Scalability

Minimal implementation of the Mimblewimble protocol. A decentralized, privacy-preserving digital currency with no addresses or amounts. Grin leverages Mimblewimble to collapse transactional data, making it fast and lightweight. Your Embassy will run a full node enabling interchangeable wallet interfaces. 

## Introduction to Mimblewimble and Grin
Simplicity
Mimblewimble is a blockchain format and protocol that provides extremely good scalability, privacy and fungibility by relying on strong cryptographic primitives. It addresses gaps existing in almost all current blockchain implementations.

Grin is an open source software project that implements a Mimblewimble blockchain and fills the gaps required for a full blockchain and cryptocurrency deployment.

Here's an overview:

- Clean and minimal implementation, and aiming to stay as such.
- Follows the MimbleWimble protocol, which provides great anonymity and scaling characteristics.
- Cuckoo Cycle proof of work.
- Relatively fast block time: one minute.
- Fixed block reward over time with a decreasing dilution.
- Transaction fees are based on the number of Outputs created/destroyed and total transaction size.
- Smooth curve for difficulty adjustments.

## Goal and Characteristics

### Privacy by Default

This enables complete fungibility without precluding the ability to selectively disclose information as needed.
Scales mostly with the number of users and minimally with the number of transactions (<100 byte kernel), resulting in a large space saving compared to other blockchains.

### Strong and proven cryptography

Mimblewimble only relies on Elliptic Curve Cryptography which has been tried and tested for decades.

### Design simplicity

Easy to audit and maintain over time

### Community driven

Community driven, encouraging mining decentralization. No ICO, Founder reward, pre-mine or dev-tax.
