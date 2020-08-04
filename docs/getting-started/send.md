
# Sending Grins

To send grins, use the `send` command, and specify both the destination address (`-d`) and the amount.

```bash
grin-wallet send -d [address] [amount]
```

Example of sending 3600 grins:

```bash
grin-wallet send -d grin1dhvv9mvarqwl6fderuxp3qgl6qppvhc9p4u24347ec0mvgg6342q4w6x56 3600
```

!!! note "Locked Outputs"
    From now on and until confirmed in the chain, the outputs used in this transcation are [locked](TODO.LOCKED-OUTPUTS-TITLE).

The wallets will interact through Tor and complete the process automatically.

However, If the connection wasn't successful, your wallet will show a `Slatepack` message which you need to provide the receiver with. The receiver then uses the `receive` command to process it (as demonstrated earlier), and returns back to you a `Slatepack` he produced himself.

Simply type the following command:

```bash
grin-wallet finalize
```
And enter into the prompt the last `Slatepack` message you were sent.

Your wallet will complete the transaction building process and post it to the network, along with a fee, to be included in an upcoming block. Celebrate this milestone with an unapologizing smile ツ
