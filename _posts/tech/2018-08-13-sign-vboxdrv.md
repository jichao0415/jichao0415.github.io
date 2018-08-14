---
layout: post
title: sign kernel modules in Ubuntu 18.04
category: tech
tags: [ubuntu, virtualbox, windows]
---
## Problem message

modprobe: ERROR: could not insert 'vboxdrv': Required key not available

## [Answer copied from website](https://askubuntu.com/questions/760671/could-not-load-vboxdrv-after-upgrade-to-ubuntu-16-04-and-i-want-to-keep-secur)

Since kernel version 4.4.0-20, it was enforced that unsigned kernel modules will not be allowed to run with Secure Boot enabled. Because you want to keep Secure Boot, then the next logical step is to sign those modules.

1. Create signing keys

    ```shell
    openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=Descriptive common name/"
    ```
    Option: for additional security, skip the -nodes switch, which will ask for a password. Then before moving on to the next step, make sure to export KBUILD_SIGN_PIN='yourpassword'

2. Sign the module (vboxdrv for this example, but repeat for other modules in ls $(dirname $(modinfo -n vboxdrv))/vbox*.ko) for full functionality)

    ```shell
    sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxdrv)
    ```

    ```shell
    ls $(dirname $(modinfo -n vboxdrv))/vbox*.ko
    /lib/modules/4.15.0-30-generic/updates/dkms/vboxdrv.ko
    /lib/modules/4.15.0-30-generic/updates/dkms/vboxnetadp.ko
    /lib/modules/4.15.0-30-generic/updates/dkms/vboxnetflt.ko
    /lib/modules/4.15.0-30-generic/updates/dkms/vboxpci.ko
    sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxnetadp)
    sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxnetflt)
    sudo /usr/src/linux-headers-$(uname -r)/scripts/sign-file sha256 ./MOK.priv ./MOK.der $(modinfo -n vboxpci)
    ```


3. Confirm the module is signed

    ```shell
    tail $(modinfo -n vboxdrv) | grep "Module signature appended"
    ```

4. Register the keys to Secure Boot

    ```shell
    sudo mokutil --import MOK.der
    ```
    which will ask for a password to use to confirm the import in the next step.

5. Reboot your computer and follow instructions to Enroll MOK (Machine Owner Key). Here's a [sample](https://sourceware.org/systemtap/wiki/SecureBoot) with pictures. The system will reboot one more time.

6. Confirm the key is enrolled

    ```shell
    mokutil --test-key MOK.der
    ```

If VirtualBox still does not load, it may be because the module didn't load (sudo modprobe vboxdrv will fix that) or that the key is not signed. Simply repeat that step and everything should work fine.

Resources: Detailed website [article for Fedora](http://gorka.eguileor.com/vbox-vmware-in-secureboot-linux/) and [Ubuntu implementation](https://github.com/Canonical-kernel/Ubuntu-kernel/blob/master/Documentation/module-signing.txt) of module signing. @zwets for [additional security](https://askubuntu.com/questions/760671/could-not-load-vboxdrv-after-upgrade-to-ubuntu-16-04-and-i-want-to-keep-secur/768310#768310). @shasha_trn for [mentioning all the modules](https://askubuntu.com/questions/760671/could-not-load-vboxdrv-after-upgrade-to-ubuntu-16-04-and-i-want-to-keep-secur/768310#768310).

Additional resource: I created a bash script for my own use every time ```virtualbox-dkms``` upgrades and thus overwrites the signed modules. Check out my [vboxsign on GitHub](https://github.com/Majal/maj-scripts/blob/master/vboxsign).

