---
title: "中國金融二級市場行情數據的特徵"
tags: 金融 投資 數據
excerpt: 行情數據分爲兩部分：交易行情和訂單委托行情。交易行情就是交易數據；訂單委托行情就是買賣的報價及其委托量。把交易行情和訂單委托行情結合在一起就叫做 TAQ (Trades and Quotes) 行情。而其中，中國的金融二級市場行情數據分爲兩個等級：Level-1 和 Level-2。不同的金融品類、不同的行情數據等級均有不同的特徵，包括時延、周期、數據完整度等均有區別。本文就詳細地介紹了其中的區別。
---

*Tips: 以下内容如無補充默認是在討論中國大陸的金融二級市場行情數據，默認省略“中國”或“中國大陸”修飾前綴，請自行腦補。本文尚未更新完畢，你可以持續關注。*



## 前置概念

要把行情數據的特徵交待清楚，首先要明确以下前置概念：



### Trades and Quotes

行情數據分爲兩部分：

- 交易行情 (Trades): 内含了交易數據
- 訂單委托行情 (Quotes): 買賣的報價及其委托量

把交易行情和訂單委托行情結合在一起就叫做 TAQ (Trades and Quotes) 行情。



### Level-1 and Level-2

行情數據分爲兩個等級：

- Level-1
- Level-2

這兩個等級在不同的交易所、不同的品類上的特徵是不相同的：其時延、周期、數據完整度等均有區別。

但是相同的地方也有：Level-2 往往設有門檻，比如需要**付費獲取**等。



### Datafeed

行情數據分爲展示型和非展示型 (Datafeed)。

一般來説，Datafeed 都是 Level-2 級別的數據，和展示型行情數據從内容上來看是一致的，區別僅僅是獲取方式和許可的使用方式。

總體來説，Datafeed 是用作投資研究、程序化交易，而展示型則僅能在網頁或者行情軟件中用作展示之用。具體細節上各家交易所的要求可能有所區別，可以到他們的網站上獲得詳細資料。

以上証所爲例，其官網上説明如下：

> 上证所Level-2行情非展示数据（原称Datafeed，以下简称非展示数据）是指上证所信息网络有限公司（以下简称信息公司）提供的、以特定接口方式提供给客户、供其在指定机房内进行非展示使用的Level-2行情数据。
>
> 非展示使用包括以下两种许可用途：
>
> 1.自用:许可客户在指定机房内的自有应用系统中使用非展示数据，包括用于会员客户交易行为监测监控系统优化、期权做市等。
>
> 2.转发:包括机房内转发和接口转发。
>
> 机房内转发：许可客户在自用的基础上，在同一指定机房内转发其用户，供用户部署在同一指定机房内的应用系统使用。
>
> 接口转发：许可客户使用经我司认可的转发系统，以接口方式转发其用户，用户可不限定在指定机房内使用。

來源：[上证所信息网络有限公司](https://ic.sseinfo.com/page/business/ywsq_level2Datafeed.jsp)



### Tick and Snapshot

在交易行情數據中，可以有兩種分類：

- 逐筆行情 (Tick)
- 快照行情 (Snapshot)

Tick 數據是指完整記錄了市場所有交易撮合信息的數據，是最精細最完整的行情數據。

*這裏提一句，往往有混淆 Tick 數據與 Snapshot 數據的情況，要區分的話簡單來説，凡是有固定周期長度的：1秒、3秒之類的數據都不是 Tick 數據。*

Snapshot 數據是對 Tick 數據按照固定的周期進行切片后獲得的數據，是一個在時間截面上統計到的數據，損失了很多細節，不是對市場的所有交易撮合信息的完整記錄。



## 證券市場

中國大陸一共有兩個證券交易所：上海證券交易所和深圳證券交易所。

*順便一提，如果大陸居民要交易港股，有兩種方式：通過大陸交易所提供的「滬港通」、「深港通」進行交易，或者辦理境外的證券帳戶直接交易港股美股，這兩種交易方式受到的監管法規和准入門檻均有較大區別。*

### Level-1

證券交易所的 Leve-1 行情通過交易所信息技術公司的高速地面網和寬帶廣播衛星系統發布或互聯網和專線傳輸。上交所和深交所的 Level-1 特徵是一致的：

 - 都提供了3秒週期的 Snapshot TAQ 數據；
 - 為了彌補 Snapshot 數據造成的「失真」，會在 TAQ 數據中附加「當日最高價」、「當日最低價」；
 - 在 TAQ 數據中委託行情的檔位為 5 檔。

### Level-2

Level-2 數據在基礎的 Level-1 行情的基礎上增加了增值信息，需要付費來取得行情推送。在這個級別上，兩所交易所的數據特徵有了一些區別：

#### 深交所

在基本行情的基礎上，在 TAQ 數據中委託行情的檔位由 5 檔擴張至 10 檔，並提供最優檔位的前 50 筆訂單的委託量，快照週期仍然為 3 秒。

然後附加提供了毫秒級的 Quotes Tick 信息以及 Trades Tick 信息。

#### 上交所

上交所提供的 Level-2 行情的 TAQ 數據中與深交所一致。

另外附加提供了毫秒級的 Trades Tick 信息。


## 期貨市場

*# TODO*