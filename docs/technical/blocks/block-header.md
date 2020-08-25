### Block Header

General format of a **Block Header** in a MimbleWimble/Grin:

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| version   | Version of the block | 2 bytes  |
| height   | Height of this block since the genesis block (height 0)  | 8 bytes  |
| timestamp   | Timestamp at which the block was built | 8 bytes  |
| prev_hash   | Hash of the block previous to this in the chain | 32 bytes  |
| prev_root   | Root hash of the header MMR at the previous header | 32 bytes  |
| output_root  | Merkle root of all the commitments in the TxHashSet | 32 bytes  |
| range_proof_root | Merkle root of all range proofs in the TxHashSet | 32 bytes  |
| kernel_root | Merkle root of all transaction kernels in the TxHashSet | 32 bytes  |
| total_kernel_offset | Total accumulated sum of kernel offsets since genesis block. | 32 bytes  |
| output_mmr_size | Total size of the output MMR after applying this block. | 8 bytes  |
| kernel_mmr_size | Total size of the kernel MMR after applying this block. | 8 bytes  |
|   |   | Sub-Total: **226** bytes  |
| - | - | - |
| pow  | Proof of work data | For cuck(AR)oo 29: 174 bytes.<br>For cuck(AT)oo 31: 184 bytes.<br>For cuck(AT)oo 32: 189 bytes. |
|   |   | Total:<br> **400** bytes for C29<br>or **410** bytes for C31<br> or **415** bytes for C32  |

### PoW
General format of a **Proof Of Work** in a MimbleWimble/Grin:

| Field        | Description           | Size  |
|:-------------|:-------------|:-----|
| total_difficulty   | Total accumulated difficulty since genesis block | 8 bytes  |
| secondary_scaling   | Variable difficulty scaling factor for secondary proof of work  | 4 bytes  |
| nonce | Nonce increment used to mine this block. | 8 bytes  |
| proof   | Proof of work data | cuckoo_size: 1byte<br>cuckoo_solution of C29: 42&lowast;29 bits=153 bytes<br>cuckoo_solution of C31: 42&lowast;31 bits=163 bytes<br>cuckoo_solution of C32: 42&lowast;32 bits=168 bytes  |
|   |   | Sub-Total: **174** bytes for C29 and **184/189** bytes for C31/C32  |


## Example
Here is a **principle example** of a MimbleWimle/Grin block header. The related block is included in Mainnet chain block: [1869](https://grinscan.net/block/1869).

| Offset        | Field           | Hex Value  |
|:-------------|:-------------|:-----|
|   0   | version               | 0001 |
|   2   | height                | 000000000000074d |
|  10   | timestamp             | 000000005c3ff107 |
|  18   | prev_hash             | 00001915c97bfad5bf9bb30a1035771446770502a937bd16a8c8a8f1fc6db730 |
|  50   | prev_root             | 2eb356e36c03f16796dd4a6eef8cbcbeabaade84eecf013516367b376d646db7  |
|  82   | output_root           | b56ccc990181d04d57585d3f0163b4f636fe3208a024596d14221bfa7739e693 |
| 114   | range_proof_root      | a341b87862f430f9dda5fd0b88be175b6cc81b5e84bf6882d98cc31750b5001a |
| 146   | kernel_root           | b3e9708378ef256d479c6b166ba0a56c26d1b2e985c5e48065dbaa8a1fa6e606 |
| 178   | total_kernel_offset   | 0755f37fa67bc3d3f0a7eddec7117013be42afff3602e7480d7271fff92763f1 |
| 210   | output_mmr_size       | 0000000000001445 |
| 218   | kernel_mmr_size       | 0000000000001130 |
| 226   | total_difficulty      | 00000108e24af3bb |
| 234   | secondary_scaling     | 00000473 |
| 238   | nonce                 | 712493895227b883 |
| 246   | edge_bits             | 1d |
| 247   | edge_nonces           | 43c8d2e098ba28f4f3748aab7d8be1aab3353259e6c6d4c046293f1c2f92afe7c5868bc5007fc51bae789ab3776d7efefb71547144b70ae8e764921bad8cb821a0c5d2adb455e1c406e1b2e39a2db51d7c552e4c1dd78bde1eea91f0843da6b3f1cb86e4ccb93371452b66c0ab292cd42525bcbf832fe358a3b22bcf7615e8397516ad8135a709213a77347cf4de104df5585984df0246fa03 |
| 400 |

## Description

To reproduce the proof of work for block 1968 with the [reference cpu mean solver](https://github.com/tromp/cuckoo/blob/master/src/cuckaroo/mean.cpp) available as a [grin-miner plugin](https://github.com/mimblewimble/grin-miner/tree/master/cuckoo-miner/src/cuckoo_sys/plugins), compile with HEADERLEN=246 and run

```text
$ ./mean29x8 -x 0001000000000000074d000000005c3ff10700001915c97bfad5bf9bb30a1035771446770502a937bd16a8c8a8f1fc6db7302eb356e36c03f16796dd4a6eef8cbcbeabaade84eecf013516367b376d646db7b56ccc990181d04d57585d3f0163b4f636fe3208a024596d14221bfa7739e693a341b87862f430f9dda5fd0b88be175b6cc81b5e84bf6882d98cc31750b5001ab3e9708378ef256d479c6b166ba0a56c26d1b2e985c5e48065dbaa8a1fa6e6060755f37fa67bc3d3f0a7eddec7117013be42afff3602e7480d7271fff92763f10000000000001445000000000000113000000108e24af3bb00000473712493895227b883 -n 659720595 -s
Looking for 42-cycle on cuckaroo29("",659720595) with 50% edges
Using 3752MB bucket memory at 106e55000,
1x25MB thread memory at 1f1695000,
8-way siphash, and 128 buckets.
nonce 659720595 k0 k1 k2 k3 2a19d2a799213637 34da44e4123eb76 aaf4eb8132d503a 25af7154b3a0c87c
genUVnodes size 536870912 rdtsc: 11582318731
sortVnodes round  1 size 339366261 rdtsc: 12444604916
trimedges id 0 round  3 size 94071091 rdtsc: 4322291767
trimedges id 0 round  7 size 26425030 rdtsc: 864460691
trimrename id 0 round 14 size 8139955 rdtsc: 425070799 maxnnid 31733
trimrename id 0 round 15 size 7201653 rdtsc: 444656879 maxnnid 27950
trimedges1 id 0 round 31 size 1906932 rdtsc: 35389089
trimedges1 id 0 round 63 size 497051 rdtsc: 22820057
trimrename1 id 0 round 66 size 454675 rdtsc: 26397569 maxnnid 1942
trimrename1 id 0 round 67 size 441696 rdtsc: 23213276 maxnnid 1854
  6-cycle found
  8-cycle found
  2-cycle found
  32-cycle found
  42-cycle found
matchUnodes id 0 rdtsc: 9459309257
findcycles rdtsc: 10050627872
Time: 18049 ms
Solution d2c843 145d4c7 29d3cfd 316fb57 35b3aae 3732c99 51b0353 5e387e5 5e7af92 62c5c36 6f15fc0 734f15c 7e6d77b a38fdff add11c5 c9cfd01 cad1b92 d010dc4 d2b74b1 d89c2ab e3b2e10 eda96cd 10b955f0 117ae3a9 11ea1ede 11ec2784 12fc6ce9 1399c90d 1457133b 15e03315 1750b0a6 17f784a4 18e32f83 195d951a 1a055db3 1a2cea73 1a73581a 1b9d1084 1bd1f0d1 1ea9a21b 1f845958 1fd23016
Verified with cyclehash 9c50298a86c18d6660a65f94759354b4ca27122d2486cad224bbb86f347a35fb
1 total solutions
```

Note that this solver overwrites the last aligned 4 bytes of the header with the provided nonce, so we pass the decimal equivalent of 0x27528993. Finally, note that this solver computes cyclehashes on the uncompressed edge nonces whereas Grin computes it on the 153 bytes (with 0 bit padding) compressed edge nonces.
