---
title: "毒L紀 程設一[4] HW5心得 - Sanitizer啊  哪次不Sanitize"
date: 2020-12-11T11:48:09+08:00
lastmod: 2020-12-11T11:48:09+08:00
description: "安利compiler sanitizer免未定義侵擾你"
tags : ["Computer Programming I", "毒L紀", "師大", "救援"]
layout: post
type:  "post"
highlight: false
draft: false
---

# 前言

這次的作業很~~歡樂~~的跟陣列有關，但眾所週知週知， C 語言的陣列若超界存取只會導致未定義行為，而沒有語言層的邊界檢查，這在撰寫複雜的邏輯時可能會難以追縱問題的來源，今天來安利個神兵利器。

# Sanitizer 消毒劑 (X

## `-fsanitize=undefined`

未定義行為~~消毒劑~~，他可以幫你抓出各種未定義行為，包含無窮遞迴、 NULL Pointer Dereference 、陣列超界存取等⋯⋯祝你除百毒 \( ?

## `-fsanitize=leak`

幫你抓記憶體洩漏，功能我覺得跟 Valgrind 不相上下，而且可以即時抓到記憶體洩漏，更加方便 ！

缺點是要 link `-lusan` 函數庫！

## `-fsanitize=address`

這個可以幫你抓到不合法的記憶體存取 ( 也包含陣列存取超界 ) ，不過更為強大！

缺點也一樣，需要 link `-lasan` 函數庫！

## 參考 Makefile

有鑒於有些選項需要 Link Library ，底下提供我使用的 Makefile ！

```makefile
CC=gcc
CFLAGS=-Wall -Wextra -std=c11
LDFLAGS=-lm 

TARGETS=hw0501 hw0502 hw0503 hw0504 hw0505
hw0501_OBJ=hw0501.o basic.o sort.o
hw0502_OBJ=hw0502.o basic.o matrix.o
hw0503_OBJ=hw0503.o basic.o polynominal.o
hw0504_OBJ=hw0504.o basic.o analyze.o
hw0505_OBJ=hw0505.o basic.o minesweeper.o terminal.o queue.o

.PHONY: all

all: CFLAGS:=$(CFLAGS) -O2 
all: $(TARGETS) 

debug: CFLAGS:=$(CFLAGS) -g -DDEBUG -fsanitize=leak -fsanitize=undefined
debug: LDFLAGS:=$(LDFLAGS) -fsanitize=address -lubsan -lasan 
debug: $(TARGETS)

dev: CFLAGS:=$(CFLAGS) -g -DDEBUG
dev: LDFLAGS:=$(LDFLAGS) 
dev: $(TARGETS)

.SECONDEXPANSION:
$(TARGETS): $$($$@_OBJ)
	$(CC) $^ -o $@ $(LDFLAGS)

%.o: $@.c

clean:
	rm -rf $(TARGETS) $(foreach targ,$(TARGETS),$(foreach obj, $($(targ)_OBJ), $(obj)))
```

