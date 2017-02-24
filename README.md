AnthillaOS
==========

[![Join the chat at https://gitter.im/Anthilla/AnthillaOS](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Anthilla/AnthillaOs?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

AnthillaOS, Appliance Operating System </br>
- [GitHub Repository](https://github.com/Anthilla/AnthillaOS) is for documentation and linking with other projects.

AnthillaOS can be used to build a normal, standalone appliance, </br> or can be used to build structured and/or destructured Clusters in High Availability and Fault Tolerance

Introduction
------------

Anthilla OS aims to be a complete Linux System image to serve HA and FT services distributed and replicated (virtualized or not) with a primary focus on bypass the real problem of any "System" (System in general sense),the drift towards chaos. 

This, simply *Using* and without structural modification or package forks or custom developments, we want to use a normal Linux (recompiled *Gentoo*, with *Genkernel* kernel images, like *System Rescue CD*, or *Sabayon*) System, as custom Linux embedded System *readonly* as possible, separing configurations, and Data leaving System image untouched.

Using heavily *Systemd* and looking to *CoreOS* and *Android*, *CyanogenMod*, or *Network Appliances*
Custom application can be added (we suggest, as additional image) on the Boot Volume *BootExt* and loaded at boot with some pluggable runlevel (thanks *systemd*)

- System Image, 
- Kernel package, 
- Firmwares, 
- Data,
- Configurations, 

are separed, like an Android system on a smartphone. But is a pure Linux, based on Gentoo, rebuilded with different USE flags looking to *Arch Linux* and *Sabayon*.

the *APPS* can be loaded dinamically making a compressed squash package as "standalone deploy" and simply mounting it in read only, and read/write directories are created and mounted separately in a simply structured filesystem Layout similar to *Android*

We work usually to write *APPS* in dotnet, c# firstly, and run through MONO compiled from latest git tree, and system-coherent, always present in the System Image, we also preparing Apps Templates related to antd project, the system Manager for AnthillaOS

AnthillaOS + antd aims to be a plug and play environment for standalone application, managed by systemd as process.

*A note*: please, share with us use cases, and configuration, to tune and make AnthillaOS more powerful and clean.

Thanks for attention and Enjoy.

[Anthilla](http://www.anthilla.com) Team

Description
-----------

Usable test/dev exercise for appliances.

An x86-64bit Gentoo image customized for readonly usage as appliances, fully functional and complete of near 1200 pkg enclosing Gentoo distro in squash files + boot loader

AnthillaOS project use intensively "mount" command to "overmount" files and directory (with -o bind option) to *attach* files to the read only system inage, and to make updates easy and non-bloking as possible.

The system always boot in a known state; 
then starts an our service through systemd, and mount extended targets (runlevels) to run kernel related Units and application Units

Focusing the work on pure web Apps, it's present a Target level for websockets to integrate services and applications to get they  web-aware

For Apps we suggest the use of *NancyFX* and *mono*
about the command available on the system, we refer on linux kernel officially related packages and structures.
- pseudo filesystems /sys /proc to "speak with the Kernel" on the fly, 
- iproute2 for the network, 
- tc for traffic control, 
- nftables for the firewall

but near 1200 packages are available.
such as: systemd, bird routing daemon, openvswitch, zfs, qemu(kvm), libvirt, gluster, docker, nginx, php, go-lang ...

some packages like NetworkManager or other are present, but not normally used and their use is discouraged, 
will be removed in future releases.

any *Apps* who need to work with the *system* have to use a definite *cli command line*, like a operator have to do
the usage need to be clean as possible, without dependenciecies on system specific libs or system objects, but simply *use* the system. or directly or by systemd units, also for oneshot executions.

Available as VM Image .qed (qemu, xen, kvm, virtualbox) it is ready to be used as vm or to be dumped to and usb key:

- Download it; 
- Configure the "aos_DATE_vhd001.*" as first virtual hard disk of a vm;
- Boot it;
- Log In with user: "root" and password: "root";
- Access also from network, dhcp on eth0 and sshd are enabled by default.

The content of: *aos_DATE_vhd001.qed*

- System Image in: *System/aos_DATE_System.squashfs.xz* - Readonly OS img.
- Kernel in: Kernel directory.
- Looks and Make Attention (!!!) to the Partition layout, and *BootExt* Volume Structure.
- DIRS directory it's made to contain your images and directories to Overmount to default one or new one.
    - Please maintain the Syntax: DIR_path + extension (!!!) for example:
        - example 1: ```DIR_lib64_firmware.squashfs.xz``` *-Mount On->* ```/lib64/firmware``` (part of kernel pkg in Kernel directory, on BootEx).
        - example 2: ```DIR_etc_ssh``` *-Mount On->* ```/etc/ssh ```.

    - *DIR_* can be the prefix for directories(compressed or uncompressed) and *FILE_* for single files always in _DIRS_ directory on *BootExt*.

## Why is important to maintain this syntax? because we are working to *automate* it!

- Default automated mountpoints by [genkernel-next](https://github.com/Sabayon/genkernel-next) :
    - ```/mnt/cdrom``` : *BootExt* Volume: (__"Boot Volume"__ and the __"System Repository"__)
    - ```/mnt/livecd``` : mounted Active System Image ```/System/aos_DATE_System.squashfs.xz```
    - ```/mnt/overlay``` : Aufs auto overlay (it's a tmpfs aka *__ramdisk__* that permit to "change" as read/write image the running system) all changes not voluntary stored on /mnt/cdrom (mounted in rw) or other Partitions or Disks, *are lost on reboot*.

- You can store on the *BootExt* volume(formatted in ext4 as default) what you need and your experiments in *BootExt* (Mounted On ```/mnt/cdrom```) structure:

- ```Apps``` : Repository for your Apps wrote in Php, Mono, Go, or any kind of present language/processor available 
- ```DIRS``` : Repository for Custom *DIRS* and *FILES*, *NOT DATA(s)*
- ```Kernel``` : Kernel Repository, *a softlink* called ```active-kernel``` and ```active-initrd``` respectively to kernel and initrd define the boot kernel, the same for ```recovery-kernel``` and ```recovery-initrd```
- ```Overlay``` : Repository for a aufs custom overlay if is needed
- ```Scripts``` : Repository for your scripts
- ```SecureBoot``` : SecureBoot, Thanks [Sabayon Linux](https://www.sabayon.org/)!!!!
- ```System``` : System Repository, *a softlink* called ```active-system``` the same for ```recovery-system```
- ```Units``` : Repository for Externally called Systemd, Runlevels, and Units
- ```boot``` : link to "." for boot
- ```grub``` : The home of Grub2! here is the ```grub.cfg```, simplified.
- ```livecd``` : important empty file to permit the genkernel generated kernel to find the volume to boot

###If you want mount or look into without booting, 
take an image file, you can mount QED it with qemu-nbd command.

*Example:*
```
qemu-nbd --connect=/dev/nbdX aos_DATE_vhd001.qed
kpartx -a /dev/nbdX
blkid #looks /dev/mapper/nbdXp3 BootExt
mount LABEL=BootExt /mountpoint -o rw,noatime,discard
```

Mirrors
-------

Our development "nightlies" and "unstable release" Repository .
http://srv.anthilla.com/

Anthilla OS Repository Mirrors:
Cleaned and niglty replicated in Rsync.

HTTP:
http://ftp.tsukuba.wide.ad.jp/Linux/anthilla/
http://ftp.nluug.nl/os/Linux/distr/anthilla/
http://ftp.cc.uoc.gr/mirrors/linux/anthilla/
http://ftp.belnet.be/anthilla.com/

FTP:
ftp://ftp.tsukuba.wide.ad.jp/Linux/anthilla/
ftp://ftp.nluug.nl/os/Linux/distr/anthilla/
ftp://ftp.cc.uoc.gr/mirrors/linux/anthilla/
ftp://ftp.belnet.be/mirror/anthilla.com/


Inspiration
-----------
We use monolithic images of customized gentoo since 2010, for High Availability installations, in mission critical environment, where a System Incoherence (aka lib64 and compilation flags disalignment inside the binary package or after an update) can rappresent a stability issue.

##Initially inspired by:
- [System Rescue CD](http://www.sysresccd.org/SystemRescueCd_Homepage)
- Cisco Ios, as imaged system, with links/copy that points to a loadable config repository
- [PF Sense](https://www.pfsense.org/) as a good way to produce an appliance
- [Elektra](http://www.freedesktop.org/wiki/Software/Elektra/) as Configuration Repository
- [Sabayon](https://www.sabayon.org/)
- [VM Ware History](http://en.wikipedia.org/wiki/VMware#History)

##And following the natural evolution of I.C.T Scenario with:
- [CoreOS](https://coreos.com) ...was born from...
- [ChromeOS](http://en.wikipedia.org/wiki/Chrome_OS#History) ...was born from (following the same building principles of AnthillaOS) ...
- [Gentoo](https://www.gentoo.org/) The "third tree" of Linux Distros after Debian and RedHat system structure, 

##Our Methods
- We suggest you experiment with configurations and non-traditional methods to use the system without changing the System image, keeping the minimal boot to last runlevel (to be sure the system has booted) and then start what you want with controlled scripts or applications, following a quote.. "let's try to use a telephone pbx , to handle rail traffic".
- [SkunkWorks Team methods](http://en.wikipedia.org/wiki/Skunkworks_project)
- Do you want to change anything? Overmount, from ```/mnt/cdrom/DIRS/DIR_path_your_dir``` and reload services;
  - think to leave system image untouched... experiment with mount, ```mount -t tmpfs```, ```mount -o bind``` on *files and dirs*... and jump readonly limits, and automate it with runlevels and [Antd Project](https://github.com/Anthilla/Antd); 

### All Open Source components from other projects are released under respective licenses, all parts, files, subprojects developed as part or this project are released under the BSD 3 clause license.

## Features

  - Squash xz compressed (so read only) booting image;
  - Build to run with [Antd](www.anthilla.com/en/projects/antd/) [Antd on Github](https://github.com/Anthilla/Antd) 
  - [ZFS](http://zfsonlinux.org/) a part of [OpenZFS project](http://open-zfs.org/wiki/Main_Page);
  - Samba [Samba Gentoo ebuild](https://packages.gentoo.org/package/net-fs/samba) 
  - Linux Kernel 4.4.16 aufs from gentoo sources with powerful configuration (most builtin);
  - [Systemd](http://www.freedesktop.org/wiki/Software/systemd/) with minimal set of services, plus a set of custom runlevel that permits to load external defined services during boot (from git master) with a lot of features including:
      - Systemd internal container system;
      - machinectl to control containers and virtualization through libvirt;
      - some useful links to virtualize and playing with Systemd containers;
      - [\#1](http://0pointer.net/blog/systemd-for-administrators-part-xxi.html);
      - [\#2](https://www.variantweb.net/blog/systemd-machinectl-vs-docker/);
      - [\#3](http://seanmcgary.com/posts/run-docker-containers-with-systemd-nspawn);
      - [\#4](http://www.freedesktop.org/software/systemd/man/systemd-nspawn.html);
      - [\#5](https://coreos.com/docs/launching-containers/launching/getting-started-with-systemd/);
      - [\#6](https://fedoraproject.org/wiki/Features/SystemdLightweightContainers/);
      - [\#7](https://lwn.net/Articles/572957/);
      - [\#8](http://manpages.ubuntu.com/manpages/utopic/man1/machinectl.1.html);
  - [Docker](https://www.docker.com/); 
  - [HAProxy](http://www.haproxy.org/);
  - [Teamd](https://github.com/jpirko/libteam/tree/master/teamd) with some howtos; [\#1](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/sec-Configure_teamd_Runners.html) [\#2](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/sec-Controlling_teamd_with_teamdctl.html)  
  - [KVM, Qemu](http://wiki.qemu.org/Main_Page);
  - [Libvirt](http://libvirt.org/) ;
  - [Gluster](http://www.gluster.org/); 
  - [LXC](https://linuxcontainers.org/);
  - [CoreOS](https://coreos.com/) [etcd](https://coreos.com/docs/distributed-configuration/getting-started-with-etcd/) & [fleet](https://coreos.com/using-coreos/clustering/);
  - a lot of high availability and semplified deploying method included and working out of the box;
  - fully rebuilded and coherent linux system;
  - <1000 pkg installed, system size: 2.8Gb uncompressed, compressed 647mb;
  - Grub2 with a simplyfied maintenance;
  - innovative and particular tools included, filesystems, networking;
  - latest releases or git versions;
  - zram and tmpfs usage;
  - php5.6 and Mono 4.2 included (ready to be an appliance);
  - fully 64bit System;
  - fully a standard Gentoo compiled and boxed system, cleaned (/etc/portage, ports and /var/db/pkg) are separated as DEV pkg;
  - git included;
  - boot with dhcp enabled for eth0, *user: root, password: root*;
  - do you want to change anything? Overmount, from ```/mnt/cdrom/DIRS/DIR_path_your_dir``` and reload services;
  - think to leave system image untouched... experiment with mount, ```mount -t tmpfs```, ```mount -o bind``` on *files and dirs*... and jump readonly limits, and automate it with runlevels and [Antd Project](https://github.com/Anthilla/Antd); 
  - looks for ```tips.*.tips``` files for summary description in the ```/mnt/cdrom``` structure.

## Collaborate with us, or make requests, or communicate with us 

Contact us by email ->  osdev@anthilla.com

AND / OR

[![Join the chat at https://gitter.im/Anthilla/AnthillaOS](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Anthilla/AnthillaOs?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

AND / OR

contact us in *IRC* on *freenode* channel, *\#anthilla*

## Change requests to other projects

- [Genkernel Next](https://github.com/Sabayon/genkernel-next);
- in particular: [Discussion about changing mount point names](https://github.com/Sabayon/genkernel-next/issues/27 write by me,Dorvan).

