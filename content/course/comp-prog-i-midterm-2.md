---
title: "毒L紀 程設一[??] 期中考反省文2—懷疑Build System前先懷疑你寫的垃圾Source"
date: 2020-11-11T15:58:48+08:00
description: "反省Makefile啊QQQQQ"
keywords: []
draft: false
tags: ["Computer Programming I", "毒L紀", "師大", "心得"]
math: false
toc: false
---

# 心得

今天我開始好好檢視「傳說中」爛掉的 Makefile 與 Sources ，反覆思考總覺得那個 Error Message 很奇怪，明明出錯的檔案裡面有複數個 Symbol ，但報錯的總是特定的那幾個！在有了上次 Cmake 壞掉的經驗後，我決定往 Sources 出錯的方向做調查。

後來仔細檢查之後才發現....我有一個Symbol實作被我刪掉了，我卻沒有發現...，所以Linking Stage時有使用該函數的實作就會出錯 QwQ

以後，**如果 Build System 爛掉，先懷疑自己寫的程式爛掉再說了！**

# 心得—補

然後我在Linux上面跑的時候又壞掉了，所以後來就更新了 Makefile

看來是`clang`跟`gcc`在處理 linker flag 的方式不同，而 GNU make 預設處理 `LDFLAG` 的方法是`gcc`無法正確傳遞給 linker 的格式，才導致考試時 Link 會失敗，但我自己在 Mac OS 上測是好的的問題。

# Makefile Demo

附上被我證明依舊好用的 Makefile

```makefile
CC=gcc
CFLAGS=-Wall -Wextra -std=c11 -O2
LDFLAGS=-lm

TARGETS=mid01 mid02 mid03 mid04
mid01_OBJ=mid01.o basic.o
mid02_OBJ=basic_math.o mid02.o basic.o
mid03_OBJ=mid03.o basic.o
mid04_OBJ=mid04.o basic.o

.PHONY: all

all: $(TARGETS) 

.SECONDEXPANSION:
$(TARGETS): $$($$@_OBJ)
	$(CC) $^ -o $@ $(LDFLAGS)

%.o: $@.c

clean:
	rm -rf $(TARGETS) $(foreach targ,$(TARGETS),$(foreach obj, $($(targ)_OBJ), $(obj)))
```

裡面的 `TARGETS` 跟`<TARGET>_OBJ`都可以依照source file的狀況修改