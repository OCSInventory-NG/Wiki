Getting drives serial number and firmware revision can be useful when making the inventory of the
hardware you have. Getting drives info is done now by reading the associated file in /sys or /proc.
Unless you apply a special patch to your kernel, you can't get the serial number or firmware revision
of your drive that way.

## Udev

Since 2.6, udev is the new way of populating the /dev directory with the connected drives or devices.
Information can be found in the /dev/.udev/db files or using the udevinfo command. My macmini drive
would return :

    $ cat /dev/.udev/db/\\x2fblock\\x2fsda
    N:sda
    S:disk/by-id/scsi-1ATA_FUJITSU_MHV2060BHPL_NW84T6225N37
    S:disk/by-id/ata-FUJITSU_MHV2060BHPL_NW84T6225N37
    S:disk/by-path/pci-0000:00:1f.2-scsi-0:0:1:0
    M:8:0
    E:DEVTYPE=disk
    E:ID_VENDOR=ATA
    E:ID_MODEL=FUJITSU_MHV2060B
    E:ID_REVISION=0081
    E:ID_SERIAL=1ATA_FUJITSU_MHV2060BHPL_NW84T6225N37
    E:ID_SERIAL_SHORT=ATA_FUJITSU_MHV2060BHPL_NW84T6225N37
    E:ID_TYPE=disk
    E:ID_BUS=scsi
    E:ID_ATA_COMPAT=FUJITSU_MHV2060BHPL_NW84T6225N37f
    E:ID_PATH=pci-0000:00:1f.2-scsi-0:0:1:0
<br/>

    $ udevinfo -a --path /sys/block/sda

    Udevinfo starts with the device specified by the devpath and then
    walks up the chain of parent devices. It prints for every device
    found, all possible attributes in the udev rules key format.
    A rule to match, can be composed by the attributes of the device
    and the attributes from one single parent device.

      looking at device '/block/sda':
        KERNEL=="sda"
        SUBSYSTEM=="block"
        DRIVER==""
        ATTR{dev}=="8:0"
        ATTR{range}=="16"
        ATTR{removable}=="0"
        ATTR{size}=="117210240"
        ATTR{stat}=="   79591    16624  2034515   871732    51121   403019  3635616 12201712        0   553652 13097052"
        ATTR{capability}=="12"

      looking at parent device '/devices/pci0000:00/0000:00:1f.2/host2/target2:0:1/2:0:1:0':
        KERNELS=="2:0:1:0"
        SUBSYSTEMS=="scsi"
        DRIVERS=="sd"
        ATTRS{device_blocked}=="0"
        ATTRS{type}=="0"
        ATTRS{scsi_level}=="6"
        ATTRS{vendor}=="ATA     "
        ATTRS{model}=="FUJITSU MHV2060B"
        ATTRS{rev}=="0081"
        ATTRS{state}=="running"
        ATTRS{timeout}=="30"
        ATTRS{iocounterbits}=="32"
        ATTRS{iorequest_cnt}=="0x1fea8"
        ATTRS{iodone_cnt}=="0x1fea8"
        ATTRS{ioerr_cnt}=="0x0"
        ATTRS{modalias}=="scsi:t-0x00"
        ATTRS{evt_media_change}=="0"
        ATTRS{queue_depth}=="1"
        ATTRS{queue_type}=="none"

Almost everything is listed but :

* capacity is missing : size in sectors can be found, but sector's size can be only got trough ioctl. Relying on a command such as fdisk seems to be the way to go.
* vendor, and bus type have to be guessed can't be used as is.

## 3ware

Info (model serial capacity firmware) from hard drives on 3ware controllers can be obtained from the
tw_cli command. A serial number can be obtained for units too, this value can be compared to the ones
found in udev database, in fine you can tell which drives belong to sda, sdb, ...etc. Older cards do
not provide serial for units.