---
title: "第一次Hugo!"
date: 2020-08-16T01:07:37+08:00
description: "第一次使用Hugo採雷心得文"
tags: ["Hugo", "BlogHosting", "Web", "心得"]
math: false
toc: false
---
一開始因為某部落格主從Jekyll跳槽Hugo，讓我注意到了Hugo，那時我就下定決心以後寫blog一定要用Hugo。時隔多月，我已經要開始寫blog了，便著手研究如何舒服調教Hugo。
# 安裝
一開始都很順利，隨便選了一個Terminal的theme。 結果因為他要我安裝babel等npm套件而卡住，後來才知道把在hugo的根目錄用npm安裝所需套件即可。
# 選Theme
這真的很難，一開始選的Terminal因為等寬字體的關係，閱讀起來其實有點吃力，看久了不舒服，所以就想打掉重練好好換個theme。左思右想後選了這個Codex，雖然是邪教Light Theme但版面大方俐落，選用閱讀起來舒服的字體，在多方比對後終於定稿！
# 調教
有了第一次調教Temrinal theme的經驗，對於hugo的結構與環境也熟悉了不少，而且這次的theme貼心提供主頁，讓我喇喇設定檔就有好看的主頁可以用 : )

唯一想要許的願望大概就是希望可以串插件讓我中文字與英文字的交接處可以自動加入空格了。這等著以後研究吧，先睡。

## 驚魂
然後正當我想安詳的睡去時，才發現我只有輸出XML檔。後來爬了好久才發現我剛才創建gh-pages這個production分支的時候，誤刪了作為theme本體的submodule的內容了，所以沒辦法找到theme的相關設定，只好輸出xml檔案。

重新部屬submodule後事情就安心結束了

# Github Action自動部屬
然後就迎來了精彩的Github Action自動部屬了，早上我簡單的喇了一下文件跟組成Github Action的部件後，發現其實各個元件都包的很好了。

我便從[GH Action Hugo](https://github.com/peaceiris/actions-hugo)的網頁裡面喇了範例出來用，結果居然爛掉！

後來才發現因為`gh-pages`這個branch已經存在，我把它刪掉讓GH Action重建這個branch就好了