---
title: "Linux 邏輯磁區在重開後變為 inactivate"
date: 2020-08-26T15:38:57+08:00
description: "處理跨PV的lvm LV在重開後變為inactive"
tags: ["心得", "事件", "linux", "lvm"]
draft: false
Toc: true
---

# Info

## 硬體

**CPU:** AMD Ryzen 5800X

**GPU:** AMD RX570

**RAM:** 16GB

## 磁區分配

### PV

| 硬碟    | SSD(nvme0n1p5) | HHD(sda2) |
| ------- | -------------- | :-------- |
| PV size | 250GB          | 250GB     |

### VG

**SSD+HHD:** sys

### LV

**sys/root**(根目錄) : SSD 186GB

**sys/home**(家目錄) : SSD 64GB(Lvm Cache) + HHD 250GB

### 非LVM

1. **nvme0n1p1 ( 500MB ):** EFI
2. **nvme0n1p4 ( 1GB ):** /boot 

## 軟體

**OS:** Linux Mint 20

*其餘軟體預載*

# 事件

系統正確設置安裝後，開機時會卡在mounting /dev/sys/home，timeout後無法正確啟動

## 研究

插入開機碟後發現`sys/home`不可見，隨後經過googling與探索後發現是不知為何LV inactivated了

研究變朝向**開機後誰把lvm磁區inactivate**了！

一開始找到了一篇[文章](https://serverfault.com/questions/777872/home-logical-volume-is-not-available-after-reboot)，不但沒有用也沒解釋太多原理。

後來找到了2016年的CentOS資料，指出是未給予正確核心啟動參數所致，詳見[原文](https://unix.stackexchange.com/questions/213027/lvm-volume-is-inactive-after-reboot-of-centos)，詳細嘗試與研究後發現與這個無關(Ubuntu似乎不會理會這個parameter)。

之後轉而研究核心啟動參數與`init.d`和systemd等啟動時的管理器，觀察lvm2 service的生命週期但一無所獲，尤其是無法發現lvm service有任何行為異常。

之後陸續試了修改initramfs-tool跟重裝lvm等方法，但依舊失敗。

## 解

隔天經過調閱syslog發現LVM在discover PV VG LV時出現了以下行為

Load /dev/nvme0n1 ->

PV Discovered (Event driven) ->

1 LV Discovered in VG (sys/root) ->

Load /dev/sda ->

PV Discovered (Event driven) ->

**VG & LV Discovery skipped**

等 最後那個是什麼，難怪sys/home會被inactive，因為他在LV discovery的階段被跳過而導致PV不足下線的啊。
之後發現`/etc/lvm/lvm.conf`中有個`event_activation`的選項，調成0(也就是關閉)，就可以解決因為事件驅動PV與VG掃描導致的奇怪inactive現象，就可以正常開機載掛跨磁區lvm了。 (汗顏QQ)