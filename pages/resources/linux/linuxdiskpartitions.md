---
layout: page 
title: Resources
permalink: /resources/linux/linuxdiskpartitions
---
# Linux Disk Partitions
**Linux actively encourages users to partition their disks.** If a user does not want to feature many partitions, Linux will at least encourage a /root and swap memory partition for the user. Linux devices can have many partitions, and understanding how partitions work is important in Linux forensics. 

### Partition Names
Partitions are represented by device files, located in the/dev directory. Devices files are files with a type. "c" indicating a character device and "b" indicating a block device. The first letter in each line of a file will display what device it is. 
  * All disks are block devices
  *IDE drives are given a device name 
    * /dev/hda to /dev/hdd
  *Once a drive is partitioned, the partitions are represented by numbers, 
    * ex. /dev/hda1, /dev/hdb3, /dev/hdd4

### Partition Types
The host will label a partition a certain kind of file system. The standard is ext2 or swap space, but even foreign file systems like Windows NFTS or Sun UFS are also labeled. 
  * A numerical code is associated with each partition type
    * ex. ext2 is labeled 0x83 and swap space is labeled 0x82 
    * Run /sbin/sfdisk -T to show partition types and their corresponding codes
  * Primary Partitions are the 4 main available partitions
    * Original partition tables installed as a part of the boot sector held space for four partition entries and still do 
    * One primary partition of the drive may be subpartitioned int a logical partition
  * Logical partitions allow for Linux users to get around the 4 partition limit 
    * The primary partition used to house the logical partitions are called extended partitions
      * Have their own filesystem type - 0x05 
      * They must be contiguous, unlike primary partitions
      * They contain a pointer next to a logical partition 
      * There is a 15 partition limit for SCSI disks and 63 partition limit for IDE disks
### Devices Numbers
Major and minor devices numbers are important, they refer to a driver and an instance/device managed by that driver typically. They serve as identifiers for the Linux kernel, and the kernel maintains a list of devices and a brief explanation of what function they server. 
  * Major number selects the device driver being called to perform the input/out function
  * Minor number is the parameter and it is up to the driver how that minor number is determined
