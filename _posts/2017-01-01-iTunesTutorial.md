---
layout: post
title: iTunes 最佳實踐
categories: [Tutorial]
---
iTunes Best Practices.

----

## 一 Apple Music 會員

**訂就完事兒了。**

## 二 透過 CD 擷取

如果有 CD 的話，可以充分利用 iTunes 內建的導入，iTunes 能夠自動從網路取回中繼信息，再掃 Cover 添加即可。

Tips:

- 多光盤將專輯名修改一致（注意光碟序號）iTunes 會自動合併，多藝術家請注意勾選「合輯」。

_推薦使用 Apple Lossless 編碼。_

## 三 透過虛擬光驅模擬 CD 擷取

- Exact Audio Copy
- X Lossless Decoder (mac only)
- DaemonTools（需付費）

不標準 .cue 的常見問題解決思路 ：

- 檢查 .cue 對應文件一致性。
- 檢查 .cue 文字編碼。(UTF-8 without BOM)

特別注意編輯 .cue 的時候在**純文本**下編輯。（不包括 win 下的記事本，推薦使用 Visual Studio Code。另，用 vi 也是好事一樁。）

如果還不能解決...：

- 嘗試自己寫一份 .cue。
- 使用下文的方法。

## 四 透過 XLD 進行擷取

> What is this?
> X Lossless Decoder(XLD) is a tool for Mac OS X that is able to decode/convert/play various 'lossless' audio files. The supported audio files can be split into some tracks with cue sheet when decoding. It works on Mac OS X 10.4 and later.
> XLD is Universal Binary, so it runs natively on both Intel Macs and PPC Macs.

_再次提醒：Apple Lossless 非常優秀，請務必使用它。_