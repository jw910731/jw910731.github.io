---
title: "Cmake Link Failed on Mac OS X"
date: 2020-11-10T00:44:37+08:00
description: "Cmake因為原始碼編寫錯誤導致的編譯錯誤，被誤以為是系統套件的不相容所致"
keywords: []
draft: false
tags: ["事件", "cpp", "cmake"]
math: false
toc: false
---

# Info

- Cmake :  ver. 3.18.4
- Mac OS X : ver. 10.15.7

# 事件

我最近開了個 Side Project ，但他的 Cmake 組建系統在 Mac OS X 上一直無法成功組建，錯誤訊息都是在連結時`ranlib`報告說發現某個特定的 Object File 中沒有 Symbol 。

一開始我便換了個平台 ( Linux 系統 ) 做組建，可以成功組建，因此我便往「 Cmake 與 Mac OS X 自帶的 Tool Chain 會有衝突」這個方向研究，事實上有找到不少資料都產生了與我類似的問題，但都沒有幫助。找到比較有關的是說在 Mac 上多執行緒的組建有蟲，沒辦法正確做 Linking ，但我試著將組建系統單執行緒化也仍然會有這個問題。

後來我轉而往我手上有的材料做調查，我細心地看了每個 Cmake 產出來的 Makefile 並了解他的架構，然後找到各個編譯出來的 Object File 做`objdump`。發現那個被`ranlib`報錯的 Object File 確實沒有 Symbol ，我一氣之下便用 ( 真的 ) gcc 來手動編譯這個檔案，卻發現我仍然得到沒有 Symbol 的 Object File ，心頭一震，我突然覺得應該是我的 Source FIle 寫爛了。

稍做比對後，我發現其他沒有報錯的檔案都沒有使用 C++ 的`template`，我往這個方向Google，果然就找到這篇[StackOverFlow](https://stackoverflow.com/questions/13269116/template-classes-multiple-file-project-how-to)了。我犯蠢把含有`template`的實作封裝在 source file 裡面，這會導致以編譯時期為運作基礎的模版系統無法正確運作，才會導致我一直在連結階段找不到 Symbol 。

# 後記與小結

以後 C++ 寫到 Template 時要特別注意，不可以把含有 Template 的函數實作寫在`cpp` source file 裡面封裝，不然會導致 template 系統無法正確在編譯時期運作，進而導致在連結時期出錯！

果然任何程式語言要能夠稱作母語果然要至少有寫專案的經驗才算！在跨平台程式在撰寫時，如果不能考慮各種平台的狀況，也至少也要能在多幾種的平台上做過測試才行呢。

希望之後可以研究出到底為什麼在 Linux 上這樣不符合標準的程式怎麼可以成功組建！