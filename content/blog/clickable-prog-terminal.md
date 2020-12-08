---
title: "在 Linux Terminal 中實作一個可以點的程式"
date: 2020-12-07T23:04:49+08:00
lastmod: 2020-12-07T23:04:49+08:00
description: "記錄我在 Linux Terminal 中實作一個可以接收滑鼠事件的程式的路程與心得"
tags : ["Computer Programming I", "心得", "師大"]
layout: post
type:  "post"
math: false
highlight: false
draft: false
---

# 前記

感謝超邪惡的程設老師跟我下戰帖，說：「我能寫出來的話，他就敢叫助教給我分」，我就趁著這股幹勁把東西弄出來了！

也感謝 StackOverflow 與[這篇資料](https://shyuanliang.blogspot.com/2010/09/)，他們在指引我將 tty 做 Non-Canonical Input 的部分有了很大的功勞，讓我不會啃文件過勞死 QwQ 。

## 前情提要

程設課這次的作業 ( hw0505 ) 是一個正常的踩地雷，要支援插旗功能，輸入以輸入座標來表達。我怎麼想怎麼覺得奇怪，踩地雷輸入座標也太難玩了吧，不是應該要支援點擊輸入才好嗎？我向老師提出這個想法，他居然回我「你如果寫出來我就叫助教給你分」，就衝著這個句話我就寫出來了這隻程式了！

# 大冒險開始

## 啃文件

### Xterm Control Sequence

我首先遇到的問題是「如何讓 terminal 回報滑鼠事件」，所有的資料都導向[這篇 Xterm Control Sequence 文件](https://invisible-island.net/xterm/ctlseqs/ctlseqs.pdf)，我乾啃文件啃了快要 2 小時即將吐血之際，我轉而投靠 Google 大法，在 StackOverflow 上面找到了讓 Terminal 依照指定標準回報滑鼠行為的 ASCII Escape 。

### termios.h

我趁著這股幹勁，試著印出 Terminal 回報回來的滑鼠行為，卻遇到問題了，linux 的 stdin 是 line buffered 的，我本來試著停止 C 語言的 IO Buffer 但並未造成期待的結果。在使用 `gdb` 做中斷後，才發現 `read` 這個 system call 在遇到 newline 字元時才會將控制權交回主程式，這代表我必須跟系統好好談談才行。

我找了快要1小時的資料才找到 `termios.h` 這個函數庫，可以將指定的 tty / stty 轉換成 serial input ，並可以關閉 `read `以 newline 字元為 flush 輸入流時機點的功能。我便開始啃 `termios.h` 的 manpage ，但啃了半個小時快放棄時，才想到之前乾啃官方文件的失敗經驗，立刻 Google 找別人的精華文，並順便偷 example code 做實驗，這個方法讓我在 1 小時後得到了豐厚的成果，我已經可以順利捕捉到滑鼠行為，並著手撰寫相對應的抽象層，方便我後續程式的撰寫！

## 實作大冒險 >w<

最一開始實作的其實是有關 `termios.h` 相關的實作，大部分都是從偷來的 example code 為基礎作修改，主要的問題是物件生命週期的掌握，我後來為了方便起見，大量使用 `static` 的 `struct` 全域變數來管理物件生命週期並避免命名污染！並且為了 terminal 不要在我程式結束後繼續傳送滑鼠事件到 stdin ，我也做了 `signal` 的 syscall 與 `atexit` 的呼叫來使正常與非正常終止時都能呼叫到停止 terminal 傳送滑鼠事件的 callback ！

接著我開始實作自己的滑鼠事件接收抽象層，我實作了一個監聽器的 pattern ，在開始監聽時要求傳入一組含由 function pointer 組成的 `struct`  ，用來描述遇到不同行為的 callback ，滑鼠事件觸發在此圓滿落幕！

我開始寫掃地雷的程式，有別於之前管理物件生命週期保守的做法，我大量使用指標與使用 Heap 空間的記憶體來降低耦合並增加彈性 ( 因為前幾段 code 都是要啃很多文件，但程式事實上的需求簡單，反觀此段需求比較複雜，耦合度不可以太高，不然會除蟲除到死 )。踩地雷場地儲存的部分因為我懶，我把所有資料以**位元壓縮**壓在一個 `int32_t` 裡面，方便擴增也免除複雜的記憶體管理問題。 ( 雖然後來只用了 7 個bit )

然後我速刻了一個 double linked list 來做我 BFS 的 queue 實作，無奈如同原始人般的 C 語言連 queue 這種基本的東西都要手刻啊 QQQQQQQ 。小心翼翼地檢查了記憶體的釋放，卻還是有些頭尾節點紀錄上的問題出現，發掘並除錯後，程式大致上大功告成！

# 後記

經過約 2 小時的細部微調與美化後，我終於算是正式完工了可以用滑鼠互動點擊的踩地雷遊戲，不得不說還蠻有成就感的。雖然腳因為 2 天都長時間坐在椅子上啃文件，出了點問題就是了 QwQ ，這個作品可說是用我的一條腿犧牲召喚而來的啊 \( 誤

寫這隻程式時也真心感覺到 Sanitizer 真的是太好用了，如果這次沒有開 Sanitizer ，我可能又要再多通靈 2 天才能寫出來囉 QwQ

## 成果炫耀 \( ?

這裡是[原始碼傳送門](https://github.com/jw910731/computer-programming/tree/I/hw05)， build 出來的 hw0505 就是這個神奇的程式了。不過目前因為 Mac 上好像只支援 SGR 的滑鼠事件格式而我懶的繼續寫了，所以目前是只能在 Linux 下的 Terminal 模擬器裡面正常運作的， Mac 可以成功建構但聽不到滑鼠事件 QQ 。想讓它支援的應該啃啃文件，讀讀我的 code 應該不難支援才對！