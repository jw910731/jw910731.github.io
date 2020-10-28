---
title: "Archlinux 初體驗"
date: 2020-10-28T07:17:30+08:00
description: ""
keywords: []
draft: true
tags: ["Linux", "ArchLinux", "心得"]
math: false
toc: false
---

# 前言

好吧，總之我就是~~腦袋撞到~~突然起意，看到某程設老師的電腦好像是 arch 誒，就來裝 ArchLinux 順便挑戰自己的 Linux 程度吧，畢竟用了 Linux 那麼久，再繼續被某 Linux Distrobution 和 snap 綁架也就太難看了。

本次安裝環境選擇安全一些的 VMWare Fusion 虛擬機，反正免費的不拿嗎？XD

- 硬碟30GB
- RAM 1024MB
- 3D Acceleration

RAM 給少是因為想裝 light 一點的的 Linux 在 Mac 上，僅是為了補足 CTF 和惡搞時需要 Linux 的需求

# 過程

本次的安裝完全以此[Arch Linux 官方文件](https://wiki.archlinux.org/index.php/installation_guide)進行

第一個遇到的問題應該是用`fdisk`切分割區拉，以前在圖形安裝環境下有Gparted等圖形介面可以用，雖然依舊有用`fdisk`手切過的經驗，可是還是會怕把分割區切爛。結果意外的還算順利，唯一遇到的的問題是解析度有點糟糕，所以常常整頁的列表會被切掉，上面的部分看不到 QQ

這次分割區配置是

| Partition | Type(MountPoint) | Size |
| --------- | ---------------- | ---- |
| /dev/sda1 | Linux(/)         | 28GB |
| /dev/sda2 | Linux Swap       | 2GB  |

接著就是安裝套件、設定`/etc/fstab`、設定時區、處理 initramfs 等繁瑣的小事了。
最後我還差點忘記安裝Bootloader，裝完還忘記做設定，真是蠢爆了， Tutorial 真的要好好看完啊！

然後我就發現我還忘了dhcpcd要裝，結果連不上網路，蠢到爆。

# 後記

整體安裝下來覺得難度沒有很大，大部分真正麻煩的事情套件管理都會處理掉，無需使用者操心，拿到整個乾乾淨淨的  Linux 心情也是很好 wwww，但對於第一次上手Linux的人來說可能會痛苦...很多。
安裝 ArchLinux 還蠻好玩的 ( 只要不要一直忘記裝套件 ) ，也發覺到之前瘋狂把 Linux 裝爛所獲得的經驗是能夠派上用場的。

真不得不說， ArchLinux 的套件庫裡的套件都是最新的，而且要什麼有什麼，絕對不缺，但套件庫的 Mirror 好像不太穩定，可能需要手動調校才行QQ