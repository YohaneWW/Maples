---
layout: post
title: macOS 重部署實踐
categories: [Development Notes]
series: macOS Notes
excerpt: rebuilt macOS.
---
* Index
{:toc}

## 緒論

众所周知，稳定性与可靠性并不能仅仅寄托于单一的实例永远不出问题，而是如何在任何可能的意外中迅速恢复到工作环境中来。

要想使自己的数位生活保持安宁，一套完整可靠的重部署方案就顯得很有必要。當然，為清除長久運行產生的殘留或錯誤的情況也是有的。

## TimeMachine（CCC）

TimeMachine 的備份是底線，是保證所有的內容完整保留的根基。

建議的操作是在一個全新的完整系統下使用「遷移助手」進行恢復，而非恢復介面直接進行還原，因為這會產生無可預料的問題。

實踐時，完整遷移會消耗相當的時間（300G，8h+），可能的原因是碎片文件過多，十分考驗 io。

在完整遷移後，應用程式並未完整恢復，與 iCloud 相關的內容也有丟失（Apple Books 本地內容），好在問題並不大，手動將能夠直接使用的應用複製即可。

Apple Music 本地數據庫與源文件得到了保留，照片同理（但在登入 iCloud 後會與雲端驗證，建議在完成以前不要進行改動操作）。

從 pkg 安裝的相當程式會出現關聯錯誤，Avid 系列（Sibelius 會丟失字庫文件），Adobe 系列（Adobe Creative Cloud 再啟不能），Logic Pro X 會丟失與音源的關聯，好在代理到美國後下載速度不錯。

App Store 應用的好處在此刻便非常明顯，即使不使用遷移，也能在 App Store 載就行。

值得注意的是，在一切妥當以前，應暫時關閉 TimeMachine，避免新的操作污染完整的結構。

另一種值得考慮的工具還有 [CCC](Carbon%20Copy%20Cloner)，但筆者並未嘗試過。

## iCloud

上雲是使資料可靠的另一種好方法，需要注意的只有兩點，一是完整遷移會保留原有的帳戶，永遠儘量**不要**沒事兒登出 iCloud 帳戶，這並不一定會造成資料的損失，但照片及其他使用 iCloud 的應用會重新驗證，在這期間，出錯的可能性十分十分高，所以除非萬不得已，請不要這樣做。

第二個指示是，因為眾所週知及雲服務本身對於各種環境的要求苛刻，請在一切未妥當之前**不要**做一些相當睿智的操作，因為這有相當的可能性將更改覆蓋到雲端正確的內容上。而雲服務是沒有備份的，一旦出錯，可能將造成資料的永久遺失。

## Work out

### Handoff 失效

被譽為玄學功能的 Handoff 無法使用是**正常事件**，但這裡仍提供一種可能的解決方案（再次**注意**不要沒事兒登出 iCloud）：

1. delete keychain’s handoff key.
2. Turn off **all** devices bluetooth, wifi, handoff and turn down.
3. Restart & turn up bluetooth, wifi, handoff.

檢查Instant Hotspot，如果能正常顯示，則基本完成。

### Apple Watch 无法解锁 Mac

小機率的玄學事件，表現為登入內能正常使用解鎖功能，但在登入介面無法使用，在[重置 Apple Watch](https://support.apple.com/en-us/HT204518) 後恢復。

### Cap 灯不亮

在「設定-鍵盤-輸入方式」中，勾選並取消「使用大寫鎖定鍵來切換...」解決。

## Config

如果有諸如 `bash`, `mpv`, `git`, `shadowsocks`, `vim` 的配置，建議使用 `git` 進行備份保存到雲端。

Brew 的備份則較為簡單，保留一份 leaves list 即可，有需要再次調用便是。

## 結語

總結下來，無論如何也沒法以非常快的速度恢復到工作狀態，再部署預計將消耗 1-2 day 的時間，所以除非事先即有充分準備及時間，盡量不要做系統級別的危險操作。

但無論如何，平時養成良好的使用習慣與預案，在怎樣的情況下，也不會手足無措了不是嗎。
