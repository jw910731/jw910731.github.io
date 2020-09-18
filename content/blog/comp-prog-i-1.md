---
title: "毒L紀 程設一[1] Hello World與如何編譯"
date: 2020-09-18T15:39:55+08:00
description: ""
draft: false
tags: ["Computer Programming I", "毒L紀", "師大課程", "救援"]
math: false
toc: false
---

# Programming

- 上課範例程式

```c
#include <stdio.h>
int main(){
  printf("Hello World!\n");
  return 0;
}
```

`main`是C語言定義的Entry Point，程式執行後會呼叫`main`。

[`printf`](http://www.cplusplus.com/reference/cstdio/printf/)是格式化輸出的函數，型別為`int printf ( const char * format, ... )`，也就是說輸入一個「格式化字串」(型別為內容為常數的字元陣列(也是指標))，以及多個格式化字串所需的參數，輸出格式化字串格式化後的結果到標準輸出流。格式化字串的格式以連結為例！

`return 0;`事實上是`main`函數回傳給系統的Exit Code，可以代表程式結束的狀況是正常還是異常，以Linux而言`0`是正常退出。

- 毒瘤程式

```c
#include <unistd.h>
int main(){
  char s[]="Hello World!\n";
  write(STDOUT_FILENO, s, sizeof(s));
  return 0;
}
```

不要問，裸的輸出Fixed Length Byte到stdout的人是瘋子。

# 編譯與執行

安利個編譯參數，如下：

```bash
gcc -O2 -std=c11 -Wall -Wextra -o <輸出檔案> <原始碼>
```

解釋個參數

- \-O2  幫你優化程式，讓你奇怪的錯誤更容易被發現
- \-std=c11 告訴編譯器你要寫的程式要用C11的標準來編譯
- \-Wall \-Wextra  叫編譯器多檢查些，但這樣就會出現程式可以編譯，卻會出現警告訊息的狀況，多出來的警告訊息就斟酌參考吧！

請記得把`<原始碼>`替換成自己source file的名稱，`<輸出檔案>`換成你想要編譯完的程式想要的名字

執行程式請給他完全路徑，若你的程式在你目前的Work Dir底下，用
```bash
./<輸出檔案>
```
就可以了

# 補充

Linux 提供程式三個I/O Stream

- stdin
	標準輸入流，用來輸入資訊。
- stdout
	標準輸出流，用來輸出資訊。
- stderr
	標準錯誤輸出流，用來輸出錯誤資訊。
在預設狀態下，標準錯誤流會導向到標準輸出流中，也就是說會跟你的程式輸出混在一起(混在一起的前提是你的程式要交替輸出資料到stdout & stderr)，你也可以重新導向這些輸出流到其他地方。但我累了之後看L紀有沒有要教XD

`printf`就是輸出格式化後的文字到stdout去！

# Linux 指令小抄

`ls` 列出目前工作目錄的內容

`cd` 切換工作目錄

`cat` 列印檔案

`gcc` C語言編譯器

`pwd` 查看工作目錄的完整路徑