### AnthillaOS
AnthillaOS pseudo repo for integration


# Description

Usable test/dev exercise for appliances

An x86 64bit gentoo image customized for readonly usage as appliances, fully functional and complete of <1000 pkg smashing down a gentoo distro.

available as VM Image .qed (qemu, xen, kvm, virtualbox), .vmdk (vmware), .vdi (virtualbox) it is ready to be used as vm or to be dumped to and usb key,

1- Download it, 
2- Configure the aos_DATE_vhd001.* as first virtual hard disk of a vm,
3- Boot it
4- Log In: user: root, password: root

If you want mount or look into without booting, take an image file,
you can mount QED it with qemu-nbd command:

qemu-nbd --connect=/dev/nbdX aos_DATE_vhd001.qed
kpartx -a /dev/nbdX
blkid #looks /dev/mapper/nbdXp3 -> BootExt
mount LABEL=BootExt /mountpoint -o rw,noatime,discard

the content of: aos_DATE_vhd001.qed

- System Image in: System/aos_DATE_System.squashfs.xz - Readonly OS img
- Kernel in: Kernel directory
- Looks and Make Attention (!!!) to the Partition layout, and BootExt Volume Structure.
- DIRS directory it's made to contain your images and directories to Overmount to default one or new one.
  Please maintain the Syntax: DIR_path + extension (!!!) for example:
  example 1: DIR_lib64_firmware.squashfs.xz -mount on-> /lib64/firmware (part of kernel pkg in Kernel directory, on BootEx)
  example 2: DIR_etc_ssh -mount on-> /etc/ssh

  DIR_ can be the prefix for directories(compressed or uncompressed) and FILE_ for single files always in DIRS directory on BootExt

  Why is important to maintain this syntax? because we are working to automate it!


WHEN YOU HAVE an booted VM with AnthillaOS,
you can remount as Read/Write the System Repository, 
(BootExt it's the "Boot Volume" and the "System Repository" mounted in /mnt/cdrom due to default genkernel-* standard)
simply launching: mount /mnt/cdrom -o remount,rw,discard,noatime

Default automated mountpoints by genkernel-next:
/mnt/cdrom: BootExt Volume: ("Boot Volume" and the "System Repository")
/mnt/livecd: BootExt:/System/aos_DATE_System.squashfs.xz
/mnt/overlay: Aufs auto overlay (it's a tmpfs aka ramdisk that permit to "change" as read/write image the running system, all changes not voluntary stored on /mnt/cdrom (mounted in rw) or other Partitions o Disks are lost on reboot.

You can store on the BootExt volume(formatted in ext4 as default) what you need and your experiments in BootExt (/mnt/cdrom) structure.

Apps -> Repository for your Apps wrote in Php, Mono, Go, or any kind of present language/processor available 
DIRS -> Repository for Custom DIRS and FILES
Kernel -> Kernel Repository, a softlink called active-kernel and active-initrd respectively to kernel and initrd define the boot kernel, the same for recovery-kernel and recovery-initrd
Overlay -> Repository for a aufs custom overlay if is needed
Scripts -> Repository for your scripts
SecureBoot -> SecureBoot, Thanks Sabayon Linux!!!!
System -> System Repository, a softlink called active-system the same for recovery-system
Units -> Repository for Externally called Systemd, Runlevels, and Units
boot -> link to "." for boot
grub -> The home of Grub2! here is the grub.cfg, simplified.
livecd -> important empty file to permit the genkernel generated kernel to find the volume to boot

all open source components from other projects are released under respective licenses, all parts, files,subprojects developed as part or this project are released under the BSD 3 clause license.


# Features

  -ZFS
  - Samba (REMOVED, Due instability and problem to automate the build of official and overlay pkg for both 4.1.x and 4.2.x) Dockerize or Lxc It!
  - kernel 4.0.0 aufs from gentoo sources with powerfull configuration (most builtin)
    systemd minimal set of services, plus a set of custom runlevel that permits to load external defined services during boot
    squash xz compressed (so read only) booting image
    Docker, HAProxy, Teamd, KVM/Libvirt, Gluster, LXC and a lot of high availability and semplified deploying method included and working out of the box.
    fully rebuilded and coherent linux system
    <1000 pkg installed, System size: 2.8Gb Uncompressed, compressed 647mb.
    grub2 with a simplyfied maintenance
    innovative and particular tools included, Filesystems, networking
    latest releases or git versions.
    zram and tmpfs usage..
    php5.6 and Mono 4.2 included (ready to be an appliance)
    fully 64bit System
    fully a standard Gentoo compiled and boxed system, cleaned (/etc/portage, ports and /var/db/pkg) are separated as DEV pkg
    git included
    boot with dhcp enabled for eth0, user: root, password: root
    do you want do change anything? Overmount, from /mnt/cdrom/DIRS/DIR_path_your_dir and reload services
    think to leave system image untouched..... experiment with mount, mount -t tmpfs, mount -o bind on files and dirs... and jump readonly limits
    looks for tips.*.tips files for summary description in the /mnt/cdrom structure.

# Change Requests, Bugs, Suggestions, or any kind of communication 

contact Us!!!!! 

# Change requests to other projects

Genkernel Next, https://github.com/Sabayon/genkernel-next
in particular, https://github.com/Sabayon/genkernel-next/issues/27 write by me (Dorvan)
