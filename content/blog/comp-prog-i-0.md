---
title: "毒L紀 程設一[0] 基本Linux開發環境設置"
date: 2020-09-16T22:55:30+08:00
description: "教你裝Linux順便傳教各種工具拉"
draft: false
tags: ["Computer Programming I", "毒L紀", "師大課程", "救援"]
math: true
toc: false
---
# 垃圾話

毒L紀 = 毒瘤+Linux紀
意指我以毒瘤的稱號~~讚嘆~~這個老師足夠狂，敢搬這麼毒瘤的東西出來給大家吸
總之這個系列不定期推出
會放點你可能必須知道的東西，也會放些不建議你抄也不太健康的東西
如何判斷不太健康，就是你感覺看不懂，貼進Google也查不太到正常的解釋，就是不健康
總之什麼都有什麼都不奇怪

# 設置Linux環境

## Virtual Machine
你的選擇有
- 大陸正版的VMware
- 免費難用的Virtual box
自由選擇
如何安裝請相信Google小姐的力量

## WSL 2
真的很香，究竟更新到2版後有什麼差請見[官方比較表](https://docs.microsoft.com/zh-tw/windows/wsl/compare-versions)
但總之建議不要用WSL 1惹L紀森77，而且那很毒瘤。
安裝教學可以Google也可以看[微軟把拔ㄞㄉ教學](https://docs.microsoft.com/zh-tw/windows/wsl/install-win10)
總之這傢伙可以給你一個Linux的Command Prompt(命令列)，而且你不需要手動分配固定大小的記憶體給他，他會自己想辦法，網路也互通，檔案可以從`/mnt`底下看到你的C槽D槽，但不太建議你把平常Linux的檔案放在Windows的檔案目錄下，不然存取檔案的效能可能會GG

## [Repl.it](https://repl.it/)

這個網站提供Online Editor與Linux Container(可以想成一個人一個隔離的指令列環境)，可以提供輕量化的Linux開發環境，如果只想寫作業不想搞Linux我個人非常推薦。學生可以去註冊Github Student Pack，可以有一年免費Premium的功能！(這不是業配，相信我>.<)

## 使用Mac OS X (咦

L紀說其實Mac OS是可以使用的沒問題，~~畢竟不是邪教Windows馬~~

所以有Mac就歡樂的使用吧OwO，記得使用`xcode-select --install`來安裝編譯工具鍊喔(內含`clang`與`make`)，但要注意編譯器與老師使用的`gcc`不一樣喔OwO，不過大致上除了指令以外沒什麼不相容的。(在mac裡面打`gcc`其實是`clang`喔XD)

# 設置編寫環境

## 原生/虛擬機環境/Mac OS X
強推**Sublime Text 3**給你舒服編譯環境
即使是Mac的使用者也可以一起來喔
這裡送上推薦必備的Plugin

- Chineese Localization
- SublimeLinter
- SublimeLinter-gcc
其中SublimeLinter-gcc在安裝時會提供預設的設定檔，可以跟著附贈的教學操作，使用預設設定檔，就可以有即時的語法檢查了
目前一鍵舒服編譯的工具目前只支援C++，就不放上來了，等我慢慢更新吧QwQ

## WSL 2
可以使用Visual Studio Code(不是Visual Studio ! ! )的[Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)來在WSL內的環境開發
有興趣的人可以研究看看，我沒在用這個寫C

# Linux 環境介紹與推薦

## 圖形環境預裝軟體

- gedit

  好用的編輯器，支援Syntax Highlight

- Gnome Terminal

  開啟Command Prompt來輸入指令的好傢伙，好好調教他啊XD

- 踩地雷＆麻將小遊戲

  童年啊XD

## 指令列觀念簡介

打開你的terminal程式，會看到shell等待接受你的指令。有個概念叫做「工作目錄」(Work Dir)，意指你目前所在的目錄，可以用`.`來相對性表示這個路徑，上一層路徑會叫做`..`，整個路徑的最頂端叫做「根目錄」，表示做`/`。

## 常用指令

- `man`

  有問題就問他，他可以回答你

  1. 指令
  2. System Call
  3. 函數庫(通常是C Standard Library)
  4. 特殊檔案
  5. 檔案格式

  8. 管理員用指令

  等等等...(上面號碼依照man page的分類編號)

  按`q`退出，按`h`給予各種幫助

  有搜尋功能，但我懶得寫 : )

- `ls`

  列出目前工作目錄的內容

- `cd`

  切換工作目錄

- `pwd`

  列出目前工作目錄的絕對路徑

- `less`

  可以翻頁檢視檔案，跟`man`一樣按`q`退出

- `cat`

  列出檔案內容(但無法翻頁)

- `clear`

  清除畫面內容

- `make`

  呼叫make建構系統，之後解釋:)

## 編輯器推坑

- Sublime Text 3

  可以依照自己心情贊助作者的半免費軟體(不付費偶爾就會跳出求贊助求乾爹的訊息)

- Visual Studio Code

  高顏值，可以安裝各種插件與設定調教，跨平台支援度很好

- Joe

  神奇的可愛editor，有友善的提示訊息與順手的快速鍵

- Vim

  太複雜了，自己研究XD

- Nano

  簡易的editor

# Migrate from C++
總之如果你之前寫了C++，有些習慣語法需要改一下
## `#include <cxxx>` -> `#include <xxx.h>`
如題，你以前`#include <cstdio>`現在要`#include <stdio.h>`
## 輸入輸出字串啥的
請愛用`scanf`取代`cin`，用`printf`取代`cout`。
`string`沒得用了去用`char`組成的陣列，算陣列長度請用`strlen`，然後要記得這個函數的時間複雜度有點糟請不要反覆呼叫！
然後不管是IO還是C String(指由char組成的陣列)都比C++難用不少，就多多體諒這個老傢伙拉'_>'

## 分配記憶體

C++有關鍵字`new`跟`delete`，物件還有解構子。C語言中的則要仰賴`stdlib.h`中的`malloc`, `calloc`, `realloc`等來分配記憶體，用`free`來釋放空間，解構子也是沒有的，需要自己多加注意。


# 後記
總之毒L紀真的頗為毒瘤，不管是裸的C還是Linux真的都比其他學校宗教正確，大家加油ㄅ QwQ
## 參考模版
裡面有些有些魔法相信各位隨著時間推移可以感受到他的力量 OwO
```cpp
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdint.h>
unsigned long long _a[] = {0xe69188e680b4e74c, 0x00000aa0bde4a881};
void sigsegv_handler(int sig){
	write(STDERR_FILENO, &_a, sizeof(_a));
	(void) signal(SIGSEGV, SIG_DFL);
}
int main(){
	(void) signal(SIGSEGV, sigsegv_handler);
	// 請在這裡開始你寫 :)
	return 0; 
}
```
