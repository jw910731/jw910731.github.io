---
title: "毒L紀 程設一[0] 垃圾話與傳教"
date: 2020-09-16T22:55:30+08:00
description: "教你裝Linux順便傳教Sublime Text3拉wwww"
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

# 設置編寫環境

## 原生/虛擬機環境
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
# Migrate from C++
總之如果你之前寫了C++，有些習慣語法需要改一下
## `#include <cxxx>` -> `#include <xxx.h>`
如題，你以前`#include <cstdio>`現在要`#include <stdio.h>`
## 輸入輸出字串啥的
請愛用`scanf`取代`cin`，用`printf`取代`cout`。
`string`沒得用了去用`char`組成的陣列，算陣列長度請用`strlen`，然後要記得這個函數的時間複雜度有點糟請不要反覆呼叫！
然後不管是IO還是C String(指由char組成的陣列)都比C++難用不少，就多多體諒這個老傢伙拉'_>'


# 後記
總之毒L紀真的頗為毒瘤，不管是裸的C還是Linux真的都比其他學校宗教正確，大家加油ㄅ QwQ
## 參考模版
裡面有些有些魔法相信各位隨著時間推移可以感受到他的力量 OwO
```cpp
#include <stdio.h>
#include <signal.h>
#include <unistd.h>
#include <stdint.h>
uint8_t _s[] = {0x4c,0xe7,0xb4,0x80,0xe6,0x88,0x91,0xe6,0x81,0xa8,0xe4,0xbd,160,10,0};
void sigsegv_handler(int sig){
	write(STDIN_FILENO, _s, sizeof(_s));
	(void) signal(SIGSEGV, SIG_DFL);
}
int main(){
	(void) signal(SIGSEGV, sigsegv_handler);
	// 請在這裡開始你寫 :)
	return 0;
}
```