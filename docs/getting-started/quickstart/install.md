# Quickstart

In this tutorial you will learn how to:

- Install Grin node and wallet
- Run a full node
- Create a wallet
- Send & receive grins

!!! tip ""
    If you prefer to use Grin with a graphical user interface, please refer to [community projects](community-projects.md).

## Requirements

grin and grin-wallet software are compatible with Linux, macOS and Windows.

It has been tested specifically on:

- Ubuntu 18.04 (on earlier versions you'll need to compile)
- macOS 10.15 Catalina
- Windows 10

## Install Grin and grin-wallet

'grin' is the node software and 'grin-wallet' is the command-line wallet.

!!! tip ""
    The following section will guide you to install using precompiled binaries.
    If you wish to build them yourself, see the [building section](https://github.com/mimblewimble/grin/blob/master/doc/build.md).

### Linux

#### Binaries

On [Grin website](https://grin.mw/download), download grin and grin-wallet.

Navigate to the directory where the files were downloaded and type (replace `$VERSION`):

```bash
sudo tar -C /usr/local/bin -xzf grin-$VERSION-linux-amd64.tar.gz --strip-components=1
sudo tar -C /usr/local/bin -xzf grin-wallet-$VERSION-linux-amd64.tar.gz --strip-components=1
```

??? warning "libncursesw error &#8628;"
    If you have the following error when you start grin:

    ```bash
    grin: error while loading shared libraries: libncursesw.so.5: cannot open shared object file: No such file or directory
    ```

    Then install `libncursesw5`:

    ```bash
    sudo apt install libncursesw5
    ```

#### Snap

While *not* recommended, since it's not a part of the official repo, it is nevertheless possible to install everything as Snap a package. To do so, type:

```bash
snap install grin
```

Note that when installed with Snap, grin-wallet is accessible using the `grin.wallet` command instead of `grin-wallet`.



### macOS

The easiest way to install grin and grin-wallet on macOS is with [homebrew](https://brew.sh).

If you do not have homebrew installed, open the "Terminal.app" and paste the following line:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

This will install homebrew on your computer. Once it's finished you can install grin and grin-wallet:

```bash
brew install grin grin-wallet
```

### Windows

On Windows, the simplest way to install is by downloading the binaries on the [Grin website](https://grin.mw/download).

We recommend that you create a directory called `grin` in `C:\Users\%USER%`. In this directory, extract both binaries `grin.exe` and `grin-wallet.exe`.
