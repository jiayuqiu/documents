# Ubuntu14.04挂载exfat格式U盘方法

问题描述：
若在挂在exFat格式的U盘时发生了错误
```
Error mounting /dev/sdb1 at /media/me/SeagateHDD: Command-line `mount -t "exfat" -o "uhelper=udisks2,nodev,nosuid,uid=1000,gid=1000,iocharset=utf8,namecase=0,errors=remount-ro,umask=0077" "/dev/sdb1" "/media/me/SeagateHDD"' exited with non-zero exit status 1:
```
则做如下操作

```
sudo apt-get install exfat-fuse exfat-utils
```
