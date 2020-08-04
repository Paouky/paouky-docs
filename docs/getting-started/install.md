# Quickstart

In this tutorial you will learn how to:

- Install Grin node and wallet
- Run a full node
- Create a wallet
- Send & receive grins

!!! tip ""
    If you want to use Grin with a graphical user interface, please refer to [community projects](community-projects.md).

## Requirements

Grin and Grin-Wallet software are compatible with Linux, macOS and Windows.

It has been tested specifically on:

- Ubuntu 18.04 (on earlier versions you'll need to compile)
- macOS 10.15 Catalina
- Windows 10

## Install Grin and Grin-Wallet

'Grin' is the node software and 'Grin-Wallet' is the command-line wallet.

!!! tip ""
    The following section will guide you to install using precompiled binaries.
    If you wish to build Grin and Grin-Wallet yourself, refer to the [building section](TODO).

### Linux

#### Snap Store

If you have Snap installed simply type:

```bash
snap install grin
```

Note that when installed with Snap, Grin-Wallet is accessible using the `grin.wallet` command instead of `grin-wallet`.

#### Manually

On [Grin website](https://grin.mw/download), download Grin and Grin-Wallet.

Navigate to the directory where the files were downloaded and type (replace `$VERSION`):

```bash
sudo tar -C /usr/local/bin -xzf grin-$VERSION-linux-amd64.tar.gz --strip-components=1
sudo tar -C /usr/local/bin -xzf grin-wallet-$VERSION-linux-amd64.tar.gz --strip-components=1
```

??? warning "libncursesw error &#8628;"
    If you have the following error when you start Grin:

    ```bash
    grin: error while loading shared libraries: libncursesw.so.5: cannot open shared object file: No such file or directory
    ```

    Then install `libncursesw5`:

    ```bash
    sudo apt install libncursesw5
    ```

### macOS

The easiest way to install Grin and Grin-Wallet on macOS is with [homebrew](https://brew.sh).

If you do not have homebrew installed, open the "Terminal.app" and paste the following line:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

This will install homebrew on your computer. Once it's finished you can install Grin and Grin-Wallet:

```bash
brew install grin grin-wallet
```

### Windows

On Windows, the simplest way to install is by downloading the binaries on the [Grin website](https://grin.mw/download).

We recommed that you create a directory called `grin` in `C:\Users\%USER%`. In this directory, extract both binaries `grin.exe` and `grin-wallet.exe`.
