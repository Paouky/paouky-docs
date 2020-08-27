# Linux

!!! tip ""
    * To use Grin with a graphical user-interface, see [community wallets](../community-wallets.md).
    * If you wish to build the binaries yourself, visit the [building section](https://github.com/mimblewimble/grin/blob/master/doc/build.md).

This document will guide you to install two binaries:

* `grin` - the Grin node.
* `grin-wallet` - the command-line wallet.

## Download

### Binaries

Go to [grin.mw/download](https://grin.mw/download) and download the tgz files of `grin` and `grin-wallet` by clicking on their respective name titles.

<p align="center">
  <img src="https://paouky.github.io/docs/assets/images/download-page.png">
</p>

#### Verify

To verify the release, type the following commands (seperately) and compare against the SHA256 HASH on the website.

```text
sha256sum grin-wallet-v4.0.0-linux-amd64.tar.gz
sha256sum grin-v4.0.2-linux-amd64.tar.gz
```



In the terminal, navigate to the directory where the files were downloaded and type:
```bash
sudo tar -C /usr/local/bin -xzf grin-$VERSION-linux-amd64.tar.gz --strip-components=1
sudo tar -C /usr/local/bin -xzf grin-wallet-$VERSION-linux-amd64.tar.gz --strip-components=1
```
*Replace `$VESION` with the appropriate version for each binary, for e.g. `v4.0.2`*

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


If you have a good Internet connection, you can help strengthen the network by keeping your PC running with Bitcoin Core and port 8333 open. Read the full node guide for details.
