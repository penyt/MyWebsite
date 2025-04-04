---
title: "建置虛擬主機"
description: "GCP建立虛擬主機（圖片較多，因為比較多截圖的關係，這篇用夜晚模式會比較好閱讀）"
date: 2024-03-22
params:
  author: PenLi
weight: -2
image: 
math: false
categories: ["虛擬主機","GCP"]
tags: ["免費的很香","靜態外部IP","SSH"]
draft: false
---
用GCP建立一個自己的虛擬主機一點都不難  
  
接下來會介紹如何獲取第一次使用GCP的免費額度，並實際建立一個新的虛擬主機🎈  

## 建立虛擬主機  
  
### 建立帳號
  
首先當然要有一個google帳號，然後用他登入 [GCP](https://console.cloud.google.com/?hl=zh-tw)  
  
然後免費試用給他按下去就對了  
  
全部同意，繼續
![](https://myrr.penli.quest/vm1-create-img%2F1.webp)  
  
步驟1/2，就跟平常辦一些帳號一樣，選一選然後繼續
![](https://myrr.penli.quest/vm1-create-img%2F2.webp)  
  
步驟2/2，就如實填，然後要填一張信用卡當作付款方式(簽帳金融卡應該也可以)，他會試刷一美金  
    
但之後有300美金所以基本上還不會扣到款，等90天結束需要付款的時候google會再問你
![](https://myrr.penli.quest/vm1-create-img%2F3.webp)  
![](https://myrr.penli.quest/vm1-create-img%2F4.webp)  
  
### 建立專案  
  
建立專案名稱，他有一些符號或大小寫有規定，就照著他的規定去設定  
![](https://myrr.penli.quest/vm1-create-img%2F5.webp)  
  
設定好讓他跑一下，建立完成會出現這個畫面，會顯示專案的資訊  
  
然後按左上角的三條橫線叫出選單  
![](https://myrr.penli.quest/vm1-create-img%2F6.webp)  
  
選擇compute engine -> VM執行個體  
![](https://myrr.penli.quest/vm1-create-img%2F7.webp)  
  
會需要啟用comput engine API，點選啟用，這裡需要等一下子  
![](https://myrr.penli.quest/vm1-create-img%2F8.webp)  
  
進到這個畫面前置作業就都完成了，可以開始新建自己的虛擬主機了  
![](https://myrr.penli.quest/vm1-create-img%2F9.webp)  
  
### 建立主機  
  
設定名稱(一樣會有一些符號的小限制)，區域選 奧勒岡州、愛荷華州、南卡羅來納州，然後機器類型選e2-micro  
  
為甚麼要選這些呢?因為這是GCP的免費限制!!可以參考[官方的文件](https://cloud.google.com/free/docs/free-cloud-features?hl=zh-tw#free-tier-usage-limits)  
  
當然，要選在亞洲也沒有不行，而且90天內我們有300美元(約台幣9,000元)，可以之後搬到新的主機就好  
  
不過不是特別大的流量的話，我是覺得地區選在美國速度也沒有很慢，這邊就自己選擇  
  
右邊橘色框框會根據你所選的虛擬主機配置實時更新，可以用那個大略估計你的預算，當然那只是預估而已!  gcp計費是用多少付多少的機制  
![](https://myrr.penli.quest/vm1-create-img%2F10.webp)  
  
然後往下，這邊會發現有自訂的選項，CPU和記憶體都可以根據自己的需要去設定，這邊我們直接使用配置好的e2-micro  
![](https://myrr.penli.quest/vm1-create-img%2F11.webp)  
  
繼續往下，我們會看到開機磁碟的選項，就和一般電腦一樣你會需要一個磁碟  
  
這邊點選 變更，然後我選ubuntu和標準永久磁碟，這邊也是根據自己的需求調整(google很好心的還有附上磁碟類型的比較)，想要直接選SSD或極端也可以  
  
大小先選10GB，一樣是根據需求調整，如果要用來架設雲端的話，可以把空間設大一點  
  
不過隨著這些改變的當然就是價錢啦!就根據需要自行評估了💸  
![](https://myrr.penli.quest/vm1-create-img%2F12.webp)  
  
選好開機磁碟後，右邊的價錢預估也會跟著變動  
![](https://myrr.penli.quest/vm1-create-img%2F13.webp)  
  
再來往下到防火牆的部分，如果你是想要架設網站，要讓其他人訪問你的網站，要把http和https打勾，這樣gcp會在你確定建立VM的時候，一併幫你把端口(port)80和443打開  
![](https://myrr.penli.quest/vm1-create-img%2F14.webp)  
確定之後按下建立  
  
等他跑一下，這邊建議他在跑的時候不要動視窗，不管是甚麼設置都一樣，耐心等一下比較不會出現未知的錯誤，也不會很久  
![](https://myrr.penli.quest/vm1-create-img%2F15.webp)  
  
接著看到主機名稱的左邊出現綠色勾勾✅就是成功建立了  
![](https://myrr.penli.quest/vm1-create-img%2F16.webp)  
  
這時候你自己的虛擬主機就建立好了!  
  
---
## 保留靜態外部IP
後面接著講一些小設定，主機設立好了還有一個很重要的東西是我們的IP位址  
    
目前設定下來我們的外部IP位址其實是動態的，意思就是在每次主機關機重啟的過程IP位址是會變動的  
  
那這個對於一些不論是增加安全性的IP白名單(VPN)或是其他網域的設定會比較麻煩  
  
這邊就示範一下如何保留靜態IP位址(當然如果喜歡動態的IP也可以維持不變，動態IP也有它的好處 eg.隱私性、費用問題等等...)  
  
首先我們先點進來我們的VM，然後點選上方的「編輯」  
![](https://myrr.penli.quest/vm1-create-img%2F17.webp)  
  
滑到下方會有一個「網路介面」把它點開  
![](https://myrr.penli.quest/vm1-create-img%2F18.webp)  
  
可以看到 外部IPv4 的地方是「臨時」  
![](https://myrr.penli.quest/vm1-create-img%2F19.webp)  
我們點選他  
  
然後選擇「保留靜態外部IP位址」  
![](https://myrr.penli.quest/vm1-create-img%2F20.webp)  
  
輸入名稱，然後按 保留  
![](https://myrr.penli.quest/vm1-create-img%2F21.webp)  
![](https://myrr.penli.quest/vm1-create-img%2F22.webp)  
  
儲存對主機的變更，之後跳出來，看到的外部IP就已經是靜態的IP了，重新開機也不會變動  
![](https://myrr.penli.quest/vm1-create-img%2F23.webp)  
  
如果不放心可以再點進去主機查看  
![](https://myrr.penli.quest/vm1-create-img%2F24.webp)  
  
---
## 變更歷史
右上角的小鈴鐺可以看到你做的每一個變更動作  
![](https://myrr.penli.quest/vm1-create-img%2F25.webp)  
  
---
## SSH 連線
再來是用SSH連線進入虛擬主機  
  
這邊用最直接的方式，透過gcp的SSH按鈕進入，點 在瀏覽器視窗中開啟  
![](https://myrr.penli.quest/vm1-create-img%2F26.webp)  
  
接著他會彈出一個視窗(如果瀏覽器有封鎖彈出式視窗要記得解除)，告訴你他正在連線  
  
如果有再彈出要允許他連線的話直接按 授權/同意/允許 就可以了(肯定是要授權才能使用他這個SSH連線方式...)  
  
這個SSH按鈕他會生成一對金鑰掛到你的虛擬主機，整個過程會慢一點，後面會再介紹別的方式連線主機  
  
最後...  
![](https://myrr.penli.quest/vm1-create-img%2F27.webp)  
出現這個視窗就是成功用SSH連線了!!  
  
底下綠色的會是user@vm-name  
  
@前面被我馬賽克的是你的使用者名稱(通常會是這個google帳號)，@後面就是你的虛擬主機名稱  
  
eg. gmail是iampenguin@gmail.com，用戶名會是iampenguin  
  
而虛擬主機是my-first-vm的話，就會顯示 <font color=#00FF00>iampenguin@my-first-vm</font>  
  
---
今天的介紹就到這裡ㄌ

by Pen