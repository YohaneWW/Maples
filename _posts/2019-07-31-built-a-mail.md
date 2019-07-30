---
layout: post
title: 滿分郵箱搭建指南
categories: [Development Notes]
excerpt: Built a perfect mail.
---
* Index
{:toc}

## Enviroments

- CentOS-x86\_64

## Post Office

郵局系統使用開源程序 `EwoMail`，可參考[官方文檔](http://doc.ewomail.com/docs/ewomail/jianjie)。

值得注意的是，請保證主機記憶體在 1G 及以上，否則可能會在安裝過程中便失敗，不過即使安裝上了，也僅僅是勉強運行，從穩定性及效率講都十分不建議這麼做。

### 除錯

- 在 Bandwagon 的機器上會出現相依關係失敗的錯誤

解決：檢查並手動安裝 `epel` 源

- WebMail 無法連接伺服器

解決：沒有正確配置頂級域名的 A 記錄

- 無法發送郵件：無法連接伺服器

解決：檢查 25 端口

	$ telnet smtp.126.com 25
	> 220

## 配置反向域名（PTR）  

一般在伺服器提供商修改，需修改與 `EwoMail` 主機一致。

## 配置 DMARC

- Type: `TXT`
- Name: `_dmarc`
- Value: `v=DMARC1; p=none`

## 配置 SPF

- Type: `TXT`
- Name: `@`
- Value: `v=spf1 a mx ip4:0.0.0.0 ~all `

值得注意的是，CloudFlare 雖然給出了 SPF 的記錄類型，但 `mail-tester` 並不會認的樣子。

## 配置 DKIM

[參考](http://doc.ewomail.com/docs/ewomail/dkim)

##  Last Step!

參考 [mail-tester](mail-tester.com) 給出的細節，作些微的調整。