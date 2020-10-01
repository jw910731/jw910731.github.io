---
date: 2020-09-23T20:58:19+08:00
title: "毒L紀 程設一[2] 建構工具"
description: "安利Makefile"
draft: false
tags: ["Computer Programming I", "毒L紀", "師大", "救援"]
math: false
toc: false
---

# Make

## Intro

Make指令會在執行時的word dir底下尋找Makefile這個檔案， 依照裡面的指示編譯你的程式，所以正確編寫Makefile就變成了建立建構系統時的一件重要的事情。

一個最簡單的Makefile大概如下

```makefile
.PHONY:
	gcc -o main main.c
```

但我個人覺得之後檔案變多，還想加各種編譯參數加入後會變得很難調教，在下面給個我個人覺得很棒的Makefile範例。

## 安利時間

make是Linux系統上常見的建構工具，有著強大的功能。這邊展示我個人調教的Makefile

```makefile
CC=gcc
CFLAGS=-Wall -Wextra -O2 -std=c11

P=main
OBJECTS=main.o

$(P): $(OBJECTS)

clean:
	rm $(OBJECTS) $(P)
```

其中要記得把`OBJECTS`變數設成自己原始檔(source)的名稱(請將副檔名改成`.o`)，若有多個檔案請以空格隔開，這樣在`make`執行時就會一併幫你組建好了。`CFLAGS`可以自己調成自己喜歡的編譯器參數，不管是享受`-O3`毒瘤的超爽加速還是想請編譯器別噴那麼多Warning都可以。

這個Makefile很短，且有很高的適應性，在期中期末即使遇到~~雞掰~~的要求也能優雅的組建。其中的編譯指令已經以`make`系統的預設建構指令隱含在裡面了，`LDFLAG`也因不需要link其他的Library而省略了。

詳細的Makefile設定方法可以看〈21世紀的C語言〉這本書，上面的Makefile也是在閱讀此書後寫的，書中講述了很多現代C語言在實戰中可以運用的技巧與如何優雅使用C語言，強烈推薦未來想繼續使用C語言的人入手！

# 讚曰

文元讚曰，感謝C語言讓我們不用寫組合語言，在此分享[Apollo-11登月導航的原始碼](https://github.com/chrislgarry/Apollo-11)，以組合語言寫成。因為有C語言的誕生，我們才能免於如同登月時的電腦工程師一般受到組合語言的摧殘。