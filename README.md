# Linux-kernel-compilation
This repository has the required details which are required to compile the linux kernel on different platforms

## QEmu installtion on Ubuntu for ARM
- `sudo apt-get update`
- `sudo apt-get upgrade`
- `sudo apt install python3-venv`
- `sudo apt install git`
- `git clone https://gitlab.com/qemu-project/qemu.git`
- `cd qemu`
- `mkdir build && cd build`
- `../configure --enable-debug --target-list=arm-softmmu,aarch64-softmmu`
    - `configure --help` for more info
- `make -j\<no of cores\>`
- `make install`

## Creating initrd (Initial RAM Disk) using busy box
initrd is an temperory file system in memory, which is used during linux startup process

## Windows Subsystem Linux
# Install required build utilities
- `sudo apt install git bc build-essential flex bison libssl-dev libelf-dev dwarves`
- `sudo apt-get install binutils-multiarch`
- `sudo apt-get install ncurses-dev` used for kernel configurations
- `sudo apt-get install alien` used for to covnert .rpm file into .deb file
# Install Cross compilation tool chain
- `sudo apt-get install gcc-aarch64-linux-gnu` AARCH64
- `sudo apt-get install gcc-arm-linux-gnueabi` AARCH32
For sanity test execute `aarch64-linux-gnu-gcc --version` for 64bit compilation or `arm-linux-gnueabi--gcc --version`
# AARCH32 Linux kernel compilation
- make distclean
- make ARCH=arm defconfig
- ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- make all
# AARCH64 Linux kernel compilation
- make distclean
- make ARCH=arm64 defconfig
- ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make all

## Steps to add debug symbol and GDB script
- `DEBUG_INFO=Y`
  - Location - `Kernel Hacking` --\> `Tracers` --\> `Compile time checks and compiler options`
- `GDB_SCRIPTS=Y`
  - Location - `Kernel Hacking` --\> `Tracers` --\> `Compile time checks and compiler options` --\> `Provide GDB scripts for kernel debugging`
