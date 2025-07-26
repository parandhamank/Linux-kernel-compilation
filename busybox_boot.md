# Step by Step procedure to boot linux kernel with busybox

## Compile linux kernel

## Compile busybox
- Checkout and extract busybox
<pre>
  tar -xf busybox-1.36.1.tar.bz2
</pre>
- Edit Makefile.config file and add -g -O0 flags as part of CFLAGS
- Compile busybox using following commands
<pre>
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- defconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- menuconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- busybox -j$(nproc)
</pre>
## Create initramfs
### Step 1 - Create minimal initramfs filesystem
- mkdir -p initramfs/{bin,sbin,etc,proc,sys,dev}
- create /init file with below code
<pre>
  #!/bin/sh
  mount -t proc none /proc
  mount -t sysfs none /sys
  echo -e "\n[OK] ARM Initramfs Booted Successfully!\n"
  exec setsid /bin/sh </dev/console >/dev/console 2>&1
</pre>
- Add execute permission for /init script using following command
<pre>
  chmod +x /init
</pre>

### Step 2 - Add busybox and symlinks to all the commands
- cd initramfs/bin and copy busybox image here
- create all the commands symlinks for ls command be like _ln -s busybox ls_
<pre>
  for cmd in sh ls echo cat mount uname sleep dmesg; do
    ln -s busybox $cmd
  done
</pre>

### Bundle busybox and create rootfs.cpio.gz image
<pre>
  cd .. # Make sure current directory is outside initramfs, means initramfs should come with ls command
  find initramfs -print0 | cpio --null -ov --format=newc | gzip -9 > rootfs.cpio.gz
</pre>

## Boot kernel on qemu
