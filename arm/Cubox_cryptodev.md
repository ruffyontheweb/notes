# Use `cryptodev-linux` in Cubox

Cubox has a Marvell crypto engine.  In order to take advange of it in
user-space, application such as `openssl` needs to compile with `cryptodev`
feature.

This document shows how to compile `cryptodev` as a module for Cubox and
recompiling `openssl` to take advantage of it.

***Requirements:***
* linux kernel [source][0] (this document uses `chroot` and cross-compilation setup)
* `cryptodev-linux` [source][1]
* `openssl` [source][2]
* debian [patch][3] for version scripts

**1. linux kernel**

Cross-compile the kernel if you need to.

**2. cryptodev-linux**

In the `chroot` environment, checkout `cryptodev-linux` and compile it.  
`# git clone https://github.com/nmav/cryptodev-linux.git`  
`# cd cryptodev-linux`  
`# export ARCH=arm`  
`# export CROSS_COMPILE=/opt/bin/arm-linux-gnueabihf-`  
`# KERNEL_DIR=/path/to/source/linux make`  
`# KERNEL_DIR=/path/to/source/linux make install`  

The modules `crypto.ko` is installed to `INSTALL_MOD_PATH` when you install
modules in the kernel compilation and modules installation step. It is in
`extra` directory.

This is the nasty part. You need to copy the `crypto` directory in the `/usr/include`
directory of the `chroot` environment to your root filesystem on the Cubox SD card.
Don't forget to add `crypto` to `/etc/modules` to load at boot time on Cubox. This
is on Cubox's root filesystem.  
`# echo "cryptodev" >>/etc/modules`

**3. openssl**

In the native Cubox--ARM environment--, apply the distro patch in the `openssl` src.
`# patch -p1 < version-script.patch`

Then you can compile and install `openssl`  
`# ./config --prefix=/usr -DHAVE_CRYPTODEV -DUSE_CRYPTODEV_DIGESTS shared`  
`# make`  
`# make install`

Reboot your Cubox.  The new system should have `/dev/crypto` device and `openssl`
uses it for md5, aes128...stuff.

[0]: https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git
[1]: https://github.com/nmav/cryptodev-linux.git
[2]: http://www.openssl.org/source/
[3]: http://anonscm.debian.org/viewvc/pkg-openssl/openssl/tags/1.0.1e-4/debian/
