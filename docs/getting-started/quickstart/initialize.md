# Initializing Node and Wallet

## Running a Grin Node

The rest of the documentation is common for Linux, macOS and Windows.

!!! note ""
    On Windows, add to PATH the directory where you installed `grin.exe` and `grin-wallet.exe`.

Running a Grin node is as simple as typing the command:
```bash
grin
```

You should see the following window:
![TUI](../../assets/images/tui.png)
Congratulations! 🎉 You are now running a Grin full node.
The initial sync might take from 30 minutes to a few hours depending on your connection speed and CPU performance.

!!! note ""
    If you can, consider opening port 3414 to allow for inbound peer connections, as it [benefits the network](TODO).


## Creating a Grin Wallet

While your node is syncing, let's initalize a new Grin wallet.

In the command prompt type the following:

```bash
grin-wallet init
```

Your wallet will now ask you to create a password. Most wallet commands will require it, so you'll be typing the password quite often.
In order to hide it, your keyboard input will not be displayed.

```text
Please enter a password for your new wallet
Password:
Confirm Password:
```

Next, the wallet will show your recovery-phrase:

```text
Your recovery phrase is:

undo execute festival romance just void custom leopard balcony trick waste castle fire master lecture ordinary million slam wise oil whisper mechanic episode room
```
This phrase is a list of 24 words which encode all the information needed to recover your wallet. If your computer breaks or your hard drive becomes corrupted, you can enter `grin-wallet init -r` and type the phrase to recover your grins.

Store it safely, preferably in a non-digital format.
