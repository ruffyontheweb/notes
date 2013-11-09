# Cross-compiling linux kernel for Cubox

I have AMD64 Debian Wheezy box but to cross-compile for ARM,
CuBox in my case, I need i386 box. Here is how I did it using
`debootstrap` and `chroot`.  You could use a virtual machine
running i386 architecture also. The steps are similar.

### 1. Setting up the 32bit environment
```
# mkdir ARM_CP  
# debootstrap --arch=i386 wheezy ARM_CP http://ftp.us.debian.org/debian  
# LANG=C chroot /path/to/ARM_CP /bin/bash  
# mount -t proc proc /proc  
# dpkg-reconfigure tzdata  
# apt-get install locales  
# dpkg-reconfigure locales  
# apt-get install git gcc build-essential bc uboot-mkimage
```

Note: Building the kernel requires `bc` for some calculation in the build script.

### 2. Cross-compiling the kernel
**2.1 Get cross-compilation toolchain from Linaro**  
Download the binary from Linaro downloads [webpage][0].
Untar it to /opt as instructed by their README.

**2.2 Get the source from github**  
* Rabeeh's [fork][1]
* vDorst's [fork][2]

**2.3 Compile the kernel**

```
# export ARCH=arm  
# export CROSS_COMPILE=/opt/bin/arm-linux-gnueabihf-  
# make cubox_defconfig
```  

If you want to customize your config with menuconfig, install `ncurses-dev`  
```
# apt-get install ncurses-dev  
# make menuconfig
# make -j4 uImage  
# make -j4 modules
```

The `uImage` is at `arch/arm/boot/`.

**2.4 Install modules**  
`INSTALL_MOD_PATH=/path/to/modules make modules_install`

### 3. Use 3.8 or later kernel
```
$ git clone https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git  
$ git checkout -b stable v3.12  
export ARCH=arm  
export CROSS_COMPILE=/usr/bin/arm-linux-gnueabihf-  
make dove_defconfig  
make menuconfig  
make  -j2 uImage  
cp arch/arm/boot/zImage arch/arm/boot/zImage.orig  
make dtbs  
cat arch/arm/boot/zImage.orig arch/arm/boot/dts/dove-cubox.dtb > arch/arm/boot/zImage  
make -j4 uImage  
make -j2 modules  
mkdir ../modules  
INSTALL_MOD_PATH=../modules make modules_install
```

### 4. Update a boot script
Usually, you only need to copy your new `uImage` file to `/boot` of your SD card.
However, if you decide to update your `boot.txt` script (e.g., changing `rootfstype`,
disable/enable console output), you need to update the boot script, `boot.scr`.

`# mkimage -A arm -O linux -T script -C none -n "uBoot commands" -d boot.txt boot.scr`

Copy this `boot.scr` to your `/boot` directory on your SD card.

### References
* [Debootstrap ArmHardFloatPort][3] by `michael.cubox`
* [Building Linux][4] wiki page on [www.solid-run.com][5]
* [uboot][6]

[0]: http://www.linaro.org/downloads/ "Linaro Download"
[1]: https://github.com/rabeeh/linux "Rabeeh"
[2]: https://github.com/vDorst/linux "vDorst"
[3]: http://www.solid-run.com/mw/index.php?title=Debootstrap_ArmHardFloatPort "Debootstrap"
[4]: http://www.solid-run.com/mw/index.php?title=Building_Linux "Building Linux"
[5]: http://www.solid-run.com/ "SolidRun"
[6]: http://www.solid-run.com/mw/index.php?title=UBoot "UBoot"
