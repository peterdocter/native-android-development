# Introduction #

The relative lack of development tools currently on Android makes it somewhat difficult to develop natively on the platform.  On the other
hand, Ubuntu provides a very rich development environment.  Luckily Ubuntu can be configured to run quite well in tandem with Android, and (I believe) can be very useful in bootstrapping a native development environment for Android.

# Installing Ubuntu on Android #

The steps required to install Ubuntu on Android include:

## Create an Ubuntu image ##

You could download a pre-generated Ubuntu disk image.  E.g. from here:

http://androlinux.com/android-ubuntu-development/how-to-install-ubuntu-on-android/

, but I prefer to build my own.

You need an existing Ubuntu system to do this.  The steps for creating an Ubuntu image are:

Note:  Most of this information was derived from:

http://androlinux.com/android-ubuntu-development/how-to-build-chroot-arm-ubuntu-images-for-android/

1) Use rootstock to create the image contents:

```
sudo apt-get install rootstock
rootstock --fqdn ubuntu --login ubuntu --password ubuntu --imagesize 4G --seed linux-image-omap
```

This should create a file similar to:

```
armel-rootfs-201105241813.tgz
```

You can add additional packages by simply adding packages to the --seed list.

2) Create the empty disk image and format it:

```
dd if=/dev/zero of=ubuntu.img bs=1MB count=0 seek=4096
mke2fs -F ubuntu.img
```

3) Now we need to mount the disk image:

```
sudo mount -o loop ubuntu.img /mnt
```

4) And finally, copy our image contents onto the disk image and unmount it:

```
sudo tar -C /mnt --preserve -xvzf armel-rootfs-201105241813.tgz
umount /mnt
```