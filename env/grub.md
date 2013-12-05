## All Things GRUB Related

Setting kernel console message to serial port
```GRUB_CMDLINE_LINUX_DEFAULT=“console=tty0 console=ttyS0,115200n8 debug ignore_loglevel”```

### GRUB and GPT
*Occasionally you need to install grub to a hard drive with GPT.*

You need to prepare the hard drive with GPT a small partition that is
set to [BIOS Boot partition][0]. Using `gdisk` to create an extra partition,
usually 1MB, at the beginning of your hard drive. Typically, the normal
partition starts at sector 2048 so you should have enough space for the
next partition and it is still aligned well. Set the type to `EF02` and
its name to something mnemonic like *"grub_bios"* or *"bios boot partition"*.
This partition starts at sector `34` and ends at `2047`.[1]

Then you can install your Linux distro that uses GRUB.


If you happen to discover that you need this partition when run into
*"fail to install grub on /dev/sda"* message during the installation process,
just switch to console to use `gdisk` to create the `BIOS Boot partition` as
described above and try to reinstall `grub`.

If you happen to have the system where you have almost everything *"working"*
except that GRUB is not installed on your awesome GPT drive, try this.

* Boot the system with a LIVE CD (Crunchbang Linux or Ubuntu..etc.)
* `chroot` to your system root file system
* Actually, you need to mount couple things before you `chroot`  
`# mount -o bind /dev /path/to/mounted/root`  
`# mount -o bind /proc /path/to/mounted/root`  
`# mount -o bind /sys /path/to/mounted/root`  

You might want to copy your LiveCD `/etc/resolv.conf` to root's `/etc`
if you need any interweb connection when you are in `chroot` environment.

In the `chroot` environment, run  
`# grub-mkconfig -o /boot/grub/grub.cfg`  
`# grub-install /dev/sda`

You might want to double-check that the boot UUID in /boot/grub/grub.cfg
matches with your boot partition's UUID.

Now you should be able to restart and boot your system with GRUB from
a GPT'ed hard drive.

References:
* [BIOS Boot partition][0]
* [GUID Partition Table][1]
* [GRUB][2]

[0]: http://en.wikipedia.org/wiki/BIOS_Boot_partition
[1]: https://wiki.archlinux.org/index.php/GUID_Partition_Table
[2]: https://wiki.archlinux.org/index.php/GRUB
