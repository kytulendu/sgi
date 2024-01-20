Mounting SGI XFS hard drive on modern Linux
===========================================

The disk need to be formatted with IRIX 6.5.23 or heigher (preferbly IRIX 6.5.30).
You can use the disk formatted with newer version of IRIX with older version,
for example, using disk formatted with IRIX 6.5.30 with IRIX 6.5.22 (even install IRIX 6.5.22 on it)

You can use PCI SCSI card like Adaptec AHA-2940 (work on 32bit PCI slot) or AHA-2930CU with
PCI-E to PCI adaptor board to connect old SCSI HDD to modern PC.
You may need to enable CSM or legacy boot support in BIOS for the card to work.

Tested on Arch Linux.

Mount IRIX disk
===============

Disk Image
----------

    fdisk -l "/home/khral/sgi_tezro_flame.img"

Use the start sector to calculate offset byte with formula `start sector * sector size`.
For example, the disk image have 266240 * 512 = 136314880 as starting offset byte.

Then setup loopback device

    sudo losetup -o 136314880 /dev/loop0 sgi_tezro_flame.img

Clear the log, need to clear it else it can't be mount.

    sudo xfs_repair -L /dev/loop0

Create mount directory

    sudo mkdir /mnt/sgi

Mount it

    sudo mount -t xfs /dev/loop0 /mnt/sgi

To unmount

    sudo umount /mnt/sgi
    sudo rmdir /mnt/sgi
    sudo losetup -d /dev/loop0

Physcial Disk
-------------

Like with disk image, just with physical disk.

    khral@AURENE:~$ sudo fdisk -l
    ...
    Disk /dev/sde: 68.37 GiB, 73407868928 bytes, 143374744 sectors
    Disk model: ST373307LC
    Geometry: 255 heads, 63 sectors/track, 8924 cylinders
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: sgi

    Device      Start       End   Sectors  Size Id Type       Attrs
    /dev/sde1  266240 143374743 143108504 68.2G  a SGI xfs     boot
    /dev/sde2    4096    266239    262144  128M  3 SGI raw     swap
    /dev/sde9       0      4095      4096    2M  0 SGI volhdr
    /dev/sde11      0 143374743 143374744 68.4G  6 SGI volume

    Partition table entries are not in disk order.

    khral@AURENE:~$ sudo losetup -o 136314880 /dev/loop0 /dev/sde
    khral@AURENE:~$ sudo xfs_repair -L /dev/loop0
    Phase 1 - find and verify superblock...
    Phase 2 - using internal log
            - zero log...
            - scan filesystem freespace and inode maps...
            - found root inode chunk
    Phase 3 - for each AG...
            - scan and clear agi unlinked lists...
            - process known inodes and perform inode discovery...
            - agno = 0
            - agno = 1
            - agno = 2
            - agno = 3
            - agno = 4
            - agno = 5
            - agno = 6
            - agno = 7
            - agno = 8
            - agno = 9
            - agno = 10
            - agno = 11
            - agno = 12
            - agno = 13
            - agno = 14
            - agno = 15
            - agno = 16
            - agno = 17
            - process newly discovered inodes...
    Phase 4 - check for duplicate blocks...
            - setting up duplicate extent list...
            - check for inodes claiming duplicate blocks...
            - agno = 1
            - agno = 11
            - agno = 10
            - agno = 15
            - agno = 4
            - agno = 6
            - agno = 7
            - agno = 5
            - agno = 8
            - agno = 9
            - agno = 0
            - agno = 13
            - agno = 12
            - agno = 3
            - agno = 14
            - agno = 2
            - agno = 16
            - agno = 17
    Phase 5 - rebuild AG headers and trees...
            - reset superblock...
    Phase 6 - check inode connectivity...
            - resetting contents of realtime bitmap and summary inodes
            - traversing filesystem ...
            - traversal finished ...
            - moving disconnected inodes to lost+found ...
    Phase 7 - verify and correct link counts...
    done
    khral@AURENE:~$ sudo mkdir /mnt/sgi
    khral@AURENE:~$ sudo mount -t xfs /dev/loop0 /mnt/sgi

To unmount use these command

    sudo umount /mnt/sgi
    sudo rmdir /mnt/sgi
    sudo losetup -d /dev/loop0


