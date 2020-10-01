---
date: 2020-10-01T15:45:12+08:00
title: "毒L紀 程設一[3] HW1心得 - MultiTarget Makefile"
description: "Multi-target Makefile"
draft: false
tags: ["Computer Programming I", "毒L紀", "師大", "救援"]
math: false
toc: false
---

# Multi-target Makefile

好吧，我總算要面對這個可怕的事情了。
如果今天一個Makefile要生成多個目標，我應該怎麼辦QwQ

後來我找了一些資料後調配了這個Makefile

```makefile
CC=gcc
CFLAGS=-Wall -Wextra -std=c11 -O2
TARGETS=hw0101 hw0102 hw0103 hw0105

.PHONY: all

all: $(TARGETS) 

.SECONDEXPANSION:
$(TARGETS): %: $$*.o

%.o: $@.c

clean:
	rm -rf $(TARGETS) $(addsuffix .o,$(TARGETS))
```

把`TARGETS`變數改成想編譯的執行檔檔名(們)，他就會去找同名的.c原始碼檔，把他編譯成對應的執行檔。

其中用到了一個叫做"Second Expansion"的功能，似乎與Makefile中變數evaluate的時機點有關，不過有些複雜，打算之後讀完再來寫一篇OwO

總之之前想得很簡單，以為可以優雅的把這個「小問題」處理掉，最後還是用了一堆沒讀熟的黑魔法。書單又增長了QwQ