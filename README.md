AnthillaOS
==========

[![Join the chat at https://gitter.im/Anthilla/AnthillaOS](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/Anthilla/AnthillaOs?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

AnthillaOS, Appliance Operating System </br>
- [GitHub Repository](https://github.com/Anthilla/AnthillaOS) is for documentation and linking with other projects.
- [SourceForge Repository](https://sourceforge.net/projects/anthillaos) is for released files of the project.

AnthillaOS can be used to build a normal, standalone appliance, </br> or can be used to build structured and/or destructured Clusters in High Availability and Fault Tolerance

Introduction
------------

Anthilla OS aims to be a complete Linux System image to serve HA and FT services distributed and replicated (virtualized or not) with a primary focus on bypass the real singlof any "System" (System in general sense),
the drift towards chaos. 

This, simply *Using* and without structural modification or package forks or custom developments, we want to use a normal Linux System, as custom Linux embedded System *readonly* as possible, separing configurations, and Data leaving System image untouched.

Custom application can be added (we suggest, as additional image) on the Boot Volume *BootExt* and loaded at boot with some pluggable runlevel (thanks systemd)

System Image, Kernel package, firmwares, Data, and configurations, are separed, like an Android system on a smartphone. But is a pure Linux, based on Gentoo, rebuilded.

A note: please, share with us use cases, and configuration, to tune and make AnthillaOS more powerful and clean.

Thanks for attention and Enjoy.

[Anthilla](http://www.anthilla.com) Team

Description
-----------

Usable test/dev exercise for appliances.

This, simply *Using* and without structural modification or package forks or custom developments, we want to use a normal Linux System, as custom Linux embedded System *readonly* as possible, separing configurations, and Data leaving System image untouched.

Custom application can be added (we suggest, as additional image) on the Boot Volume *BootExt* and loaded at boot with some pluggable runlevel (thanks systemd)

System Image, Kernel package, firmwares, Data, and configurations, are separed, like an Android system on a smartphone. But is a pure Linux, based on Gentoo, rebuilded.

An x86 64bit Gentoo image customized for readonly usage as appliances, fully functional and complete of \<1000 pkg smashing down a Gentoo distro.

Available as VM Image .qed (qemu, xen, kvm, virtualbox), .vmdk (vmware), .vdi (virtualbox) it is ready to be used as vm or to be dumped to and usb key:

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
        - example 1: ```bash DIR_lib64_firmware.squashfs.xz``` *-Mount On->* ```bash /lib64/firmware``` (part of kernel pkg in Kernel directory, on BootEx).
        - example 2: ```bash DIR_etc_ssh``` *-Mount On->* ```bash /etc/ssh ```.

    - *DIR_* can be the prefix for directories(compressed or uncompressed) and *FILE_* for single files always in _DIRS_ directory on *BootExt*.

## Why is important to maintain this syntax? because we are working to *automate* it!

- *WHEN YOU HAVE* a booted VM with AnthillaOS, you can remount as Read/Write the System Repository. 
(*BootExt* it's the "Boot Volume" and the "System Repository" mounted in /mnt/cdrom due to default genkernel-* standard)
simply launching: 

```bash
mount /mnt/cdrom -o remount,rw,discard,noatime
```

- Default automated mountpoints by [genkernel-next](https://github.com/Sabayon/genkernel-next) :
    - ```bash /mnt/cdrom``` : *BootExt* Volume: (__"Boot Volume"__ and the __"System Repository"__)
    - ```bash /mnt/livecd``` : mounted Active System Image ```bash /System/aos_DATE_System.squashfs.xz```
    - ```bash /mnt/overlay``` : Aufs auto overlay (it's a tmpfs aka *__ramdisk__* that permit to "change" as read/write image the running system) all changes not voluntary stored on /mnt/cdrom (mounted in rw) or other Partitions or Disks, *are lost on reboot*.

- You can store on the *BootExt* volume(formatted in ext4 as default) what you need and your experiments in *BootExt* (Mounted On ```bash /mnt/cdrom```) structure:

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

```bash
qemu-nbd --connect=/dev/nbdX aos_DATE_vhd001.qed
kpartx -a /dev/nbdX
blkid #looks /dev/mapper/nbdXp3 BootExt
mount LABEL=BootExt /mountpoint -o rw,noatime,discard
```

### All Open Source components from other projects are released under respective licenses, all parts, files, subprojects developed as part or this project are released under the BSD 3 clause license.

## Features

  - Squash xz compressed (so read only) booting image;
  - Build to run with [Antd](www.anthilla.com/en/projects/antd/) [Antd on Github](https://github.com/Anthilla/Antd) 
  - [ZFS](http://zfsonlinux.org/) a part of [OpenZFS project](http://open-zfs.org/wiki/Main_Page);
  - Samba [Samba Gentoo ebuild](https://packages.gentoo.org/package/net-fs/samba) (REMOVED! Due instability and problem to automate the build of official and overlay pkg for both 4.1.x and 4.2.x), Dockerize or Lxc It;
  - Linux Kernel 4.0.0 aufs from gentoo sources with powerful configuration (most builtin);
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

## Voluntary installed (no deps) Package list from World file:

- app-admin/collectd
- app-admin/eclean-kernel
- app-admin/eselect
- app-admin/fleet
- app-admin/hardening-check
- app-admin/hddtemp
- app-admin/killproc
- app-admin/lib_users
- app-admin/localepurge
- app-admin/logrotate
- app-admin/mktwpol
- app-admin/monit
- app-admin/packagekit
- app-admin/packagekit-base
- app-admin/perl-cleaner
- app-admin/python-updater
- app-admin/setools
- app-admin/sudo
- app-admin/syslog-ng
- app-admin/sysstat
- app-admin/tripwire
- app-admin/ulogd
- app-antivirus/clamav
- app-arch/arc
- app-arch/cabextract
- app-arch/cpio
- app-arch/freeze
- app-arch/lbzip2
- app-arch/lha
- app-arch/libarchive
- app-arch/lrzip
- app-arch/lz4
- app-arch/lzlib
- app-arch/lzma
- app-arch/lzop
- app-arch/mscompress
- app-arch/ncompress
- app-arch/p7zip
- app-arch/pax
- app-arch/plzip
- app-arch/sharutils
- app-arch/unarj
- app-arch/unrar
- app-arch/unzip
- app-arch/zip
- app-arch/zoo
- app-cdr/cdrtools
- app-crypt/efitools
- app-crypt/etcd-ca
- app-crypt/gnupg
- app-crypt/gpgme
- app-crypt/heimdal
- app-crypt/loop-aes-losetup
- app-crypt/mcrypt
- app-crypt/mhash
- app-crypt/p11-kit
- app-crypt/pinentry
- app-crypt/shash
- app-editors/nano
- app-editors/vim-core
- app-emulation/docker
- app-emulation/docker-compose
- app-emulation/libvirt
- app-emulation/lxc
- app-emulation/qemu
- app-eselect/eselect-awk
- app-eselect/eselect-ctags
- app-eselect/eselect-fontconfig
- app-eselect/eselect-lib-bin-symlink
- app-eselect/eselect-notify-send
- app-eselect/eselect-pinentry
- app-eselect/eselect-python
- app-eselect/eselect-sh
- app-eselect/eselect-timezone
- app-eselect/eselect-unison
- app-eselect/eselect-vi
- app-forensics/lynis
- app-misc/ca-certificates
- app-misc/editor-wrapper
- app-misc/mime-types
- app-misc/pax-utils
- app-misc/screen
- app-misc/scrub
- app-portage/conf-update
- app-portage/diffmask
- app-portage/eix
- app-portage/flaggie
- app-portage/gentoolkit
- app-portage/gentoolkit-dev
- app-portage/install-mask
- app-portage/layman
- app-portage/mirrorselect
- app-portage/pfl
- app-portage/portage-utils
- app-portage/smart-live-rebuild
- app-portage/ufed
- app-shells/dash
- app-shells/fish
- app-shells/pdmenu
- app-text/dos2unix
- dev-db/cdb
- dev-db/ctdb
- dev-db/etcd
- dev-db/libdbi
- dev-db/libdbi-drivers
- dev-db/libiodbc
- dev-db/unixODBC
- dev-lang/mono
- dev-lang/php
- dev-lang/python:2.7
- dev-libs/cyrus-sasl
- dev-libs/libev
- dev-libs/libmemcached
- dev-libs/libnl
- dev-libs/nettle
- dev-libs/npth
- dev-util/boost-build
- dev-util/bsdiff
- dev-util/catalyst
- dev-util/ccache
- dev-util/cmake
- dev-util/cppunit
- dev-util/ctags
- dev-util/dialog
- dev-util/gdbus-codegen
- dev-util/gperf
- dev-util/intltool
- dev-util/makepp
- dev-util/patchutils
- dev-util/pkgconfig
- dev-util/plan9port
- dev-util/re2c
- dev-util/systemtap
- dev-vcs/git
- dev-vcs/rcs
- mail-filter/amavisd-milter
- mail-filter/amavisd-new
- mail-filter/milter-regex
- mail-filter/spamass-milter
- mail-filter/spamassassin
- mail-mta/postfix
- media-fonts/dejavu
- media-fonts/roboto
- media-libs/harfbuzz
- media-plugins/alsa-plugins
- net-analyzer/argus
- net-analyzer/arping
- net-analyzer/fail2ban
- net-analyzer/fping
- net-analyzer/hping
- net-analyzer/iftop
- net-analyzer/iptraf-ng
- net-analyzer/jnettop
- net-analyzer/net-snmp
- net-analyzer/netcat6
- net-analyzer/netselect
- net-analyzer/nettop
- net-analyzer/nfdump
- net-analyzer/ngrep
- net-analyzer/nmap
- net-analyzer/rrdtool
- net-analyzer/scanlogd
- net-analyzer/scapy
- net-analyzer/sflowtool
- net-analyzer/snort
- net-analyzer/ssldump
- net-analyzer/tcpdump
- net-analyzer/tcpstat
- net-analyzer/traceroute
- net-dialup/freeradius
- net-dialup/linux-atm
- net-dialup/lrzsz
- net-dialup/mingetty
- net-dialup/minicom
- net-dialup/ppp
- net-dialup/pptpd
- net-dialup/rp-pppoe
- net-dns/avahi
- net-dns/bind
- net-dns/bind-tools
- net-dns/ddclient
- net-dns/dnsmasq
- net-dns/hesiod
- net-dns/idnkit
- net-dns/libidn
- net-dns/openresolv
- net-firewall/conntrack-tools
- net-firewall/ebtables
- net-firewall/ipsec-tools
- net-firewall/iptables
- net-firewall/nfacct
- net-firewall/nftables
- net-firewall/xtables-addons
- net-fs/netatalk
- net-fs/nfs-utils
- net-ftp/atftp
- net-ftp/ftp
- net-ftp/ftpbase
- net-ftp/proftpd
- net-libs/adns
- net-libs/c-client
- net-libs/daq
- net-libs/gnutls
- net-libs/libasyncns
- net-libs/libgsasl
- net-libs/libgssglue
- net-libs/libiscsi
- net-libs/libmicrohttpd
- net-libs/libmnl
- net-libs/libnet
- net-libs/libnetfilter_acct
- net-libs/libnetfilter_conntrack
- net-libs/libnetfilter_cthelper
- net-libs/libnetfilter_cttimeout
- net-libs/libnetfilter_log
- net-libs/libnetfilter_queue
- net-libs/libnfnetlink
- net-libs/libnfsidmap
- net-libs/libnftnl
- net-libs/libnids
- net-libs/liboping
- net-libs/libpcap
- net-libs/libproxy
- net-libs/librouteros
- net-libs/librsync
- net-libs/libssh
- net-libs/libssh2
- net-libs/libtirpc
- net-libs/openslp
- net-libs/socket_wrapper
- net-mail/cyrus-imap-admin
- net-mail/cyrus-imapd
- net-mail/mailbase
- net-mail/ripole
- net-mail/tnef
- net-misc/bridge-utils
- net-misc/connman
- net-misc/curl
- net-misc/dhcp
- net-misc/dhcpcd
- net-misc/dhcpd-pools
- net-misc/ifenslave
- net-misc/ipsc
- net-misc/jwhois
- net-misc/l7-filter-userspace
- net-misc/l7-protocols
- net-misc/memcached
- net-misc/netctl
- net-misc/netdate
- net-misc/netifrc
- net-misc/netkit-telnetd
- net-misc/networkmanager
- net-misc/ntp
- net-misc/openconnect
- net-misc/openssh
- net-misc/openvpn
- net-misc/quagga
- net-misc/radvd
- net-misc/scponly
- net-misc/socat
- net-misc/sshpass
- net-misc/ucarp
- net-misc/vconfig
- net-misc/vde
- net-misc/vpnc
- net-misc/vpncwatch
- net-misc/whois
- net-nds/rpcbind
- net-nds/tac_plus
- net-print/cups
- net-print/cups-filters
- net-proxy/c-icap
- net-proxy/dansguardian
- net-proxy/haproxy
- net-proxy/squid
- net-proxy/squidguard
- net-wireless/chillispot
- net-wireless/crda
- net-wireless/hostap-utils
- net-wireless/hostapd
- net-wireless/wireless-regdb
- net-wireless/wireless-tools
- net-wireless/wpa_supplicant
- sec-policy/apparmor-profiles
- sys-apps/acl
- sys-apps/apparmor
- sys-apps/apparmor-utils
- sys-apps/attr
- sys-apps/checkpolicy
- sys-apps/cracklib-words
- sys-apps/dbus
- sys-apps/debianutils
- sys-apps/dmapi
- sys-apps/dmidecode
- sys-apps/dnotify
- sys-apps/dtc
- sys-apps/ed
- sys-apps/entropy
- sys-apps/ethtool
- sys-apps/gentoo-functions
- sys-apps/gentoo-systemd-integration
- sys-apps/groff
- sys-apps/hdparm
- sys-apps/help2man
- sys-apps/hwids
- sys-apps/hwinfo
- sys-apps/hwloc
- sys-apps/ipmitool
- sys-apps/ipmiutil
- sys-apps/iproute2
- sys-apps/kexec-tools
- sys-apps/keyutils
- sys-apps/kmod
- sys-apps/lm_sensors
- sys-apps/lmctfy
- sys-apps/lsb-release
- sys-apps/lshw
- sys-apps/man-db
- sys-apps/man-pages-posix
- sys-apps/miscfiles
- sys-apps/mlocate
- sys-apps/mtree
- sys-apps/paludis
- sys-apps/pciutils
- sys-apps/portage
- sys-apps/pv
- sys-apps/rescan-scsi-bus
- sys-apps/sandbox
- sys-apps/sdparm
- sys-apps/sg3_utils
- sys-apps/shadow
- sys-apps/syscriptor
- sys-apps/systemd
- sys-apps/sysvinit
- sys-apps/tcp-wrappers
- sys-apps/timer_entropyd
- sys-apps/unscd
- sys-apps/usbmon
- sys-apps/usbredir
- sys-apps/usbutils
- sys-apps/watchdog
- sys-apps/xmbmon
- sys-auth/fprintd
- sys-auth/libfprint
- sys-auth/pam_abl
- sys-auth/pam_fprint
- sys-auth/pam_krb5
- sys-auth/pam_passwdqc
- sys-auth/pam_ssh
- sys-auth/pambase
- sys-auth/polkit
- sys-block/fio
- sys-block/gpart
- sys-block/hpacucli
- sys-block/lio-utils
- sys-block/mpt-status
- sys-block/ms-sys
- sys-block/mtx
- sys-block/nbd
- sys-block/open-iscsi
- sys-block/parted
- sys-block/raid-check
- sys-block/targetcli
- sys-block/tgt
- sys-block/thin-provisioning-tools
- sys-block/zram-init
- sys-boot/efibootmgr
- sys-boot/gnu-efi
- sys-boot/grub
- sys-boot/grub:2
- sys-boot/lilo
- sys-boot/syslinux
- sys-cluster/cluster-glue
- sys-cluster/corosync
- sys-cluster/csync2
- sys-cluster/glusterfs
- sys-cluster/libcman
- sys-cluster/openais
- sys-devel/gcc
- sys-firmware/ipxe
- sys-firmware/seabios
- sys-firmware/sgabios
- sys-firmware/vgabios
- sys-fs/btrfs-progs
- sys-fs/cachefilesd
- sys-fs/cryptsetup
- sys-fs/dfc
- sys-fs/dosfstools
- sys-fs/ecryptfs-utils
- sys-fs/encfs
- sys-fs/etcd-fs
- sys-fs/exfat-utils
- sys-fs/extundelete
- sys-fs/f2fs-tools
- sys-fs/fuse
- sys-fs/fuse-zip
- sys-fs/inotify-tools
- sys-fs/ldapfuse
- sys-fs/lessfs
- sys-fs/libfat
- sys-fs/lvm2
- sys-fs/mdadm
- sys-fs/mtools
- sys-fs/multipath-tools
- sys-fs/nilfs-utils
- sys-fs/ntfs3g
- sys-fs/ocfs2-tools
- sys-fs/progsreiserfs
- sys-fs/quota
- sys-fs/reiser4progs
- sys-fs/reiserfs-defrag
- sys-fs/reiserfsprogs
- sys-fs/safecopy
- sys-fs/shake
- sys-fs/squashfs-tools
- sys-fs/sshfs-fuse
- sys-fs/udev-init-scripts
- sys-fs/udisks
- sys-fs/xfsprogs
- sys-fs/zfs
- sys-fs/zfs-kmod
- sys-infiniband/libibverbs
- sys-infiniband/librdmacm
- sys-kernel/aufs-sources
- sys-kernel/dracut
- sys-kernel/genkernel-next
- sys-kernel/linux-firmware
- sys-kernel/spl
- sys-libs/cracklib
- sys-libs/db
- sys-libs/e2fsprogs-libs
- sys-libs/gdbm
- sys-libs/glibc
- sys-libs/ldb
- sys-libs/libstdc++-v3
- sys-libs/ncurses
- sys-libs/nss_wrapper
- sys-libs/ntdb
- sys-libs/openipmi
- sys-libs/pam
- sys-libs/readline
- sys-libs/slang
- sys-libs/talloc
- sys-libs/tdb
- sys-libs/tevent
- sys-libs/timezone-data
- sys-libs/uid_wrapper
- sys-libs/zlib
- sys-process/acct
- sys-process/audit
- sys-process/cronbase
- sys-process/fcron
- sys-process/htop
- sys-process/lsof
- sys-process/numactl
- sys-process/numad
- virtual/cdrtools
- virtual/krb5
- www-servers/nginx
