---
title: "Hime輸入法導致特定程式無法開啟"
date: 2020-09-11T00:13:47+08:00
description: "Linux輸入法整合不利導致HIME IME會導致某些程式崩潰"
draft: false
tags: ["事件", "linux"]
math: false
toc: false
---

# Info

## Linux env

- Linux Mint 20.04 + Linux Kernel 5.4.0-47-generic
- Cinnamon Desktop Environment
- Hime IME (ver. 0.9.10)
- Sublime Text 3 (Build 3211)

# 事件

某天我想寫程式的時候發現打不開Sublime Text 3了，但從command line打開卻沒跑出任何錯誤訊息。經過一波爬文後找到要用`--debug`來拿到錯誤訊息，接著就拿到

```
GLib-GObject-WARNING **: 19:44:00.522: cannot register existing type 'GtkIMContext'
GLib-CRITICAL **: 19:44:00.522: g_once_init_leave: assertion 'result != 0' failed
GLib-GObject-CRITICAL **: 19:44:00.522: g_type_register_dynamic: assertion 'parent_type > 0' failed
GLib-GObject-CRITICAL **: 19:44:00.522: g_object_new_with_properties: assertion 'G_TYPE_IS_OBJECT (object_type)' failed
```

類似這樣的訊息(上面截自[這裡](https://github.com/hime-ime/hime/issues/584#issue-440440185))

我沒太多想法，能看到的關鍵字只有`GtkIMContext`，只好往IME的方向研究。後來得到[HIME的Issue #584](https://github.com/hime-ime/hime/issues/584)，便自己編譯了一下HIME
安裝了自己編譯的HIME就奇蹟般的沒事了OwO，果然還是要"Homebrew"阿(?

