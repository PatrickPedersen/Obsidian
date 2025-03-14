```bash
1) gparted extend
2) lvextend -l +100%FREE /dev/rl/root
3) xfs_growfs /dev/mapper/rl-root
```

Useful commands:
```bash
  df -h
  lvdisplay
  fdisk -l
```
-----------------------------------------------------------
https://pocketadmin.tech/en/centos-8-extend-lvm/

```bash
$ sudo lvdisplay
  --- Logical volume ---
  LV Path                /dev/rl/swap
  LV Name                swap
  VG Name                rl
  LV UUID                toZKEu-P5oV-6WOV-026Z-eFnI-xaSP-FgEbz5
  LV Write Access        read/write
  LV Creation host, time myserver, 2021-12-03 21:28:03 +0800
  LV Status              available
  # open                 2
  LV Size                5.00 GiB
  Current LE             1280
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:1

  --- Logical volume ---
  LV Path                /dev/rl/root
  LV Name                root
  VG Name                rl
  LV UUID                S2soRE-umc7-Z6b3-i44x-TiBO-ulnk-ETgEoj
  LV Write Access        read/write
  LV Creation host, time myserver, 2021-12-03 21:28:03 +0800
  LV Status              available
  # open                 1
  LV Size                <194.00 GiB
  Current LE             49663
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0
```