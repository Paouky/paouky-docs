# Install

!!! tip ""
    * **GUI**: To use Grin with a graphical user-interface, see [community wallets](../community-wallets.md).
    * **BUILD**: If you wish to build the binaries yourself, visit the [building section](https://github.com/mimblewimble/grin/blob/master/doc/build.md).

The *Quickstart* series will guide you in your first steps of installing a Grin node, creating a wallet, and making a transaction.

We begin by downloading the precompiled binaries.

* `grin` - the Grin node.
* `grin-wallet` - the command-line wallet.

First, choose your operating system:

* [Linux](#linux)
* [macOS](#macos)
* [Windows](#windows)

---

## Linux

Go to [grin.mw/download](https://grin.mw/download) and download the tgz files of `grin` and `grin-wallet` by clicking on their name titles.

<p align="center">
  <img src="https://paouky.github.io/docs/assets/images/download-page.png" loading="lazy">
</p>


**Verify Release**

Type the following commands and compare the results against the respective SHA256 HASH displayed on the website.

```bash
sha256sum grin-wallet-$VERSION-linux-amd64.tar.gz
sha256sum grin-$VERSION-linux-amd64.tar.gz
```

*Replace `$VESION` with the appropriate version for each binary, e.g. `v4.0.2`*

Next, in the terminal, navigate to the directory where the files were downloaded and type:
```bash
sudo tar -C /usr/local/bin -xzf grin-$VERSION-linux-amd64.tar.gz --strip-components=1
sudo tar -C /usr/local/bin -xzf grin-wallet-$VERSION-linux-amd64.tar.gz --strip-components=1
```

??? note "libncursesw error &#8628;"
    If you have the following error when you start grin:

    ```bash
    grin: error while loading shared libraries: libncursesw.so.5: cannot open shared object file: No such file or directory
    ```

    Then install `libncursesw5`:

    ```bash
    sudo apt install libncursesw5
    ```

And you're done!

### Snap Store

You can also install everything as [Snap package](https://snapcraft.io/grin), however, note that you can't verify its content.

To install, type:

```bash
snap install grin
```

When installed with Snap, grin-wallet is accessible using the `grin.wallet` command instead of `grin-wallet`.

---

## macOS


The easiest way to install grin and grin-wallet on macOS is with [homebrew](https://brew.sh).

If you do not have homebrew installed, open the "Terminal.app" and paste the following line:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

This will install homebrew on your computer.

Once it's finished, you can simply install the binaries:

```bash
brew install grin grin-wallet
```

---

## Windows

Go to [grin.mw/download](https://grin.mw/download) and download the zip files of grin and grin-wallet by clicking on their name titles.

<p align="center">
  <img src="https://paouky.github.io/docs/assets/images/download-page.png" loading="lazy">
</p>

We recommend that you create a directory called `Grin` in `C:\Users\%USER%\AppData\Local`. The parent directory is where most user windows apps are installed, but if you want to put them somewhere else, that'll work too.

In this directory, extract both binaries `grin.exe` and `grin-wallet.exe`. Then, add them to your path:

```text
set PATH=%PATH%;C:\Users\%USER%\AppData\Local
```
*Remember to replace %USER%*


That's it, you're done!
