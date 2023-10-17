# Linux-kernel-compilation
This repository has the required details which are required to compile the linux kernel on different platforms

## Required libraries
- `sudo apt-get update`
- `sudo apt-get upgrade`
- `sudo apt install python3-venv`
- `sudo apt-get install git libglib2.0-dev libfdt-dev libpixman-1-dev zlib1g-dev ninja-build`
- `sudo apt-get install git-email`
- `sudo apt-get install libaio-dev libbluetooth-dev libcapstone-dev libbrlapi-dev libbz2-dev`
- `sudo apt-get install libcap-ng-dev libcurl4-gnutls-dev libgtk-3-dev`
- `sudo apt-get install libibverbs-dev libjpeg8-dev libncurses5-dev libnuma-dev`
- `sudo apt-get install librbd-dev librdmacm-dev`
- `sudo apt-get install libsasl2-dev libsdl2-dev libseccomp-dev libsnappy-dev libssh-dev`
- `sudo apt-get install libvde-dev libvdeplug-dev libvte-2.91-dev libxen-dev liblzo2-dev`
- `sudo apt-get install valgrind xfslibs-dev`
- `sudo apt-get install libnfs-dev libiscsi-dev`
- sudo apt-get install build-essential autoconf libtool cmake pkg-config python2-dev python2 python-dev-is-python3 swig3.0 libpcre3-dev libnode-dev gawk wget diffstat device-tree-compiler
- `sudo apt install git bc build-essential flex bison libssl-dev libelf-dev dwarves`
- `sudo apt-get install binutils-multiarch`
- `sudo apt-get install ncurses-dev` used for kernel configurations
- `sudo apt-get install alien` used for to covnert .rpm file into .deb file

## Install Cross compilation tool chain
- `sudo apt-get install gcc-aarch64-linux-gnu` AARCH64
- `sudo apt-get install gcc-arm-linux-gnueabi` AARCH32
For sanity test execute `aarch64-linux-gnu-gcc --version` for 64bit compilation or `arm-linux-gnueabi--gcc --version`

## QEmu installtion on Ubuntu for ARM
- `git clone https://gitlab.com/qemu-project/qemu.git`
- `cd qemu`
- `mkdir build && cd build`
- `../configure --enable-debug --target-list=arm-softmmu,aarch64-softmmu`
    - `configure --help` for more info
- `make -j\<no of cores\>`
- `make install`

## Creating initrd (Initial RAM Disk) using busy box
initrd is an temperory file system in memory, which is used during linux startup process
- `mkdir -p geninitrd/rootfs`
- `cd geninitrd`
- `wget https://busybox.net/downloads/busybox-1_36_1.tar.bz2`
- `tar -xvf busybox-1_36_1.tar.bz2`
- `cd busybox-1_36_1`
- `make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- defconfig`
- `make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- menuconfig`
    Make sure to set the option for a statically-linked BusyBox (in Settings - Build options)
- `make ARCH=aarch64 CROSS_COMPILE=aarch64-linux-gnu- install CONFIG_PREFIX=../rootfs`
- `cd ../rootfs`
- `mkdir proc sys dev etc etc/init.d usr/lib home home/root root`
- `touch etc/init.d/rcS`
- `chmod +x etc/init.d/rcS`
- update `etc/init.d/rcS` with below code
```
#!bin/sh
mount -t proc none /proc
mount -t sysfs none /sys
echo /sbin/mdev > /proc/sys/kernel/hotplug
/sbin/mdev -s
```
- `cd ..`
- `dd if=/dev/zero of=minrootfs.ext3 bs=1M count=32`
- `mkfs.ext3 minrootfs.ext3`
- `mkdir -p tmpfs`
- `sudo mount -t ext3 minrootfs.ext3 tmpfs/ -o loop`
- `sudo cp -r rootfs/* tmpfs/`
- `sudo sync`
- `sudo umount tmpfs`
- `rmdir tmpfs`
  

## Kernel Compilation
### AARCH32 Linux kernel compilation
- make distclean
- make ARCH=arm defconfig
- ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- make all
### AARCH64 Linux kernel compilation
- make distclean
- make ARCH=arm64 defconfig
- ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make all

## Steps to add debug symbol and GDB script
- `DEBUG_INFO=Y`
  - Location - `Kernel Hacking` --\> `Tracers` --\> `Compile time checks and compiler options`
- `GDB_SCRIPTS=Y`
  - Location - `Kernel Hacking` --\> `Tracers` --\> `Compile time checks and compiler options` --\> `Provide GDB scripts for kernel debugging`
 
## Qemu and GDB 
####Qemu command
`qemu-system-aarch64` -smp 2 -machine virt -m 1024 -cpu cortex-a53  -kernel Image -append 'root=/dev/vda console=ttyAMA0 nokaslr' -drive if=none,file=minrootfs.ext3,id=hd0 -device virtio-blk-device,drive=hd0 -nographic

nokaslr tells to allow kernel debugging
-s and -S is used for debugging

####GDB debug
`gdb-multiarch vmlinux`
set target remote :1234

