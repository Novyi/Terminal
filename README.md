1 Задание:
ОТВЕТ: Разрежённый файл (англ. sparse file) — файл, в котором последовательности нулевых байтов[1] заменены на информацию об этих последовательностях (список дыр).
Разреженные – это специальные файлы, которые с большей эффективностью используют файловую систему, они не позволяют ФС занимать свободное дисковое пространство носителя, когда разделы не заполнены. То есть, «пустое место» будет задействовано только при необходимости. Пустая информация в виде нулей, будет хранится в блоке метаданных ФС. Поэтому, разреженные файлы изначально занимают меньший объем носителя, чем их реальный объем.

2 Задание.

ОТВЕТ: Так как hardlink это ссылка на тот же самый файл и имеет тот же inode, то права доступа и владелец будут одни и те же.

3 Задание.

ОТВЕТ: 

vagrant@vagrant:~$ sudo fdisk -l

Disk /dev/sda: 64 GiB, 68719476736 bytes, 134217728 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x3f94c461

Device     Boot   Start       End   Sectors  Size Id Type
/dev/sda1  *       2048   1050623   1048576  512M  b W95 FAT32
/dev/sda2       1052670 134215679 133163010 63.5G  5 Extended
/dev/sda5       1052672 134215679 133163008 63.5G 8e Linux LVM


Disk /dev/sdb: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/vgvagrant-root: 62.55 GiB, 67150807040 bytes, 131153920 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/vgvagrant-swap_1: 980 MiB, 1027604480 bytes, 2007040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

4 Задание. 

ОТВЕТ:  
vagrant@vagrant:~$ sudo fdisk /dev/sda

![image](https://user-images.githubusercontent.com/91490218/146234940-61f32aa0-84e2-49c8-a4c8-c6d659e7dcdb.png)

5 Задание.

sudo sfdisk -d /dev/sdb | sudo sfdisk /dev/sdc
Checking that no-one is using this disk right now ... OK

Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Created a new DOS disklabel with disk identifier 0x213f7403.
/dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
/dev/sdc2: Created a new partition 2 of type 'Linux' and of size 510 MiB.
/dev/sdc3: Done.

New situation:
Disklabel type: dos
Disk identifier: 0x213f7403

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdc1          2048 4196351 4194304    2G 83 Linux
/dev/sdc2       4196352 5240831 1044480  510M 83 Linux

The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

6 Задание.
ОТВЕТ: 

vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md1 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
mdadm: size set to 2094080K
Continue creating array? y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.

7 Задание.
ОТВЕТ:

vagrant@vagrant:~$ sudo mdadm --create --verbose /dev/md0 --level=0 --raid-devices=2 /dev/sdb2 /dev/sdc2
mdadm: chunk size defaults to 512K
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.

8 Задание.
ОТВЕТ: 

vagrant@vagrant:~$ sudo pvcreate /dev/md0 /dev/md1
  Physical volume "/dev/md0" successfully created.
  Physical volume "/dev/md1" successfully created.

9 Задание.
ОТВЕТ: 

vagrant@vagrant:~$ sudo vgcreate vg1 /dev/md1 /dev/md0
  Volume group "vg1" successfully created

10 Задание.
ОТВЕТ: 

vagrant@vagrant:~$ sudo lvcreate -L 100M vg1 /dev/md0
  Logical volume "lvol0" created.

11 Задание.
ОТВЕТ: 

vagrant@vagrant:~$ sudo mkfs.ext4 /dev/vg1/lvol0
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 25600 4k blocks and 25600 inodes
Allocating group tables: done
Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done


12 Задание.
ОТВЕТ: 

vagrant@vagrant:~$ mkdir /tmp/new
vagrant@vagrant:~$ sudo mount /dev/vg1/lvol0 /tmp/new


13 Задание.
ОТВЕТ: 

root@vagrant:~# wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz
--2021-12-15 16:20:45--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 22843194 (22M) [application/octet-stream]
Saving to: ‘/tmp/new/test.gz’
/tmp/new/test.gz              100%[=================================================>]  21.78M   112KB/s    in 34s
2021-12-15 16:21:20 (647 KB/s) - ‘/tmp/new/test.gz’ saved [22843194/22843194]


14 Задание.
ОТВЕТ: 

![image](https://user-images.githubusercontent.com/91490218/146236105-06fd808e-5b1a-4d2e-ac1f-3fedf3d6f443.png)

15 Задание.
ОТВЕТ: 

![image](https://user-images.githubusercontent.com/91490218/146236168-66e9f9ec-d7e2-4a96-8fbe-0a9ce5576cda.png)

16 Задание.
ОТВЕТ: 

root@vagrant:~# pvmove /dev/md0 /dev/md1
  /dev/md0: Moved: 8.00%
  /dev/md0: Moved: 100.00%

17 Задание.
ОТВЕТ: 

![image](https://user-images.githubusercontent.com/91490218/146236269-80225848-fda6-4353-a12e-2e15f669292e.png)

18 Задание.
ОТВЕТ:

![image](https://user-images.githubusercontent.com/91490218/146236317-3d2a3051-7ae7-4599-ac18-55f95225785c.png)

19 Задание.
ОТВЕТ:

![image](https://user-images.githubusercontent.com/91490218/146236358-651789a6-f91a-4ff4-8aa3-4717528c05c2.png)

20 Задание

vagrant@vagrant:/$ exit
logout
Connection to 127.0.0.1 closed.
PS C:\Users\PS\project> vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Forcing shutdown of VM...









 







































