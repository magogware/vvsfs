# A Very Very Simple Filesystem

## About
This is a heavily commented, very simplistic filesystem module
for the Linux kernel. It's intended as a living tutorial of sorts
for the Linux virtual filesystem (VFS) and how filesystems work.

## Installation and compilation

To make use:
```
    make -C /usr/src/linux-headers-2.6.32-23-generic/  SUBDIRS=$PWD
```
modules`

(or just `make`, with the accompanying `Makefile`)

To load use:
```
    sudo insmod vvsfs.ko
```
(may need to copy `vvsfs.ko` to a local filesystem first)

To make a suitable filesystem:
```
    dd of=myvvsfs.raw if=/dev/zero bs=512 count=100
    ./mkfs.vvsfs myvvsfs.raw
```
(could also use a USB device etc.)

To mount use:
```
    mkdir testdir
    sudo mount -o loop -t vvsfs myvvsfs.raw testdir
```

To use a USB device, create a suitable partition on USB device (exercise for reader)
```
    ./mkfs.vvsfs /dev/sdXn
```
where sdXn is the device name of the usb drive, then do:
```
    mkdir testdir
    sudo mount -t vvsfs /dev/sdXn testdir
```

Use the file system:
```
    cd testdir
    echo hello > file1
    cat file1
    cd ..
```

Unmount the filesystem:
```
    sudo umount testdir
```

Remove the module:
```
    sudo rmmod vvsfs
```
