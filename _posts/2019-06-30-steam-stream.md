---
layout: post
title: 籍由 Hyper-V 與 Steam 進行串流
categories: [Development Notes]
series: win Notes
excerpt: Use Hyper-V and Steam to stream.
---
* Index
{:toc}

## Environment 

- Windows 10 Pro

## Intro

- Steam 提供了局域網串流工具
- Steam 提供了跨平台的客戶端
- Steam 能添加任意應用

這些特性使得通過 Steam 能夠非常方便快速的進行局域網串流。

但有一個問題，Steam 在串流過程中會佔用介面，所以通過 Windows 自帶的虛擬化工具 Hyper-V 來實現多工操作。

## 配置 Hyper-V

因為許多 gal 在 win10 下會有兼容性問題，所以選取 win7 鏡像。

### 配置虛擬交換機

如果只需要宿主與虛擬機通訊，這裡可以跳過，選取默認就好。

如果需要外部通訊或 Steam 庫在服務器或 NAS 上，這裡需要新建一個外部的交換機。

### 新建虛擬機

- 代數：第一代
- 內存：4G
- 虛擬硬碟：20G
- CPU 核心：4

在創建時掛載 win7 的安裝鏡像即可，配置完成後可以添加一個檢查點。

## 查錯

### 解決聲卡問題

因 Hyper-V 定位於生產環境，默認無法掛載聲卡。

而要使串流獲取聲音，僅需要 `Virtual Audio Canle` 虛擬出一張聲卡即可解決。

## 最佳實踐

＊虛擬機配置可按需冗余，若配置太低可能會造成串流時卡頓。

＊為達到最大效能，建議無線設備使用 5G 頻段進行連接。
