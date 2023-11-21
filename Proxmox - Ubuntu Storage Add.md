Check the available Disk Size and update the disk size
---
```Shell
cfdisk
```
> If free space is not listed, then initiate a rescan of ***"/dev/sda"*** with
> ```Shell
> echo 1>/sys/class/block/sda/device/rescan
> ```
> Once complete rerun ***"cfdisk"*** command and free space must be listed.

Resizes Physical Volume after space addition
---
```Shell
pvresize /dev/sda3
```

Extend the Logical volume to all the available space
---
```Shell
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
```

Resize ZFS pool to available space
---
```Shell
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

Check all the disk partitions and sizes
---
```Shell
df -h
```

Check Physical Volume details in case of issues
---
```Shell
pvdisplay
```

Check Volume Group details in case of issues
---
```Shell
vgdisplay
```

Check Logical Volume details in case of issues
---
```Shell
lvdisplay
```