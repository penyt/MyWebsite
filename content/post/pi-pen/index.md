---
title: "使用 Node.js 製作的樹莓派溫度監控網頁應用"
description: 我叫他 pi-pen，一個簡易快速的樹莓派溫度監控程式
date: 2025-01-14T16:22:16Z
image: 
slug: pi-pen
weight: -8
math: 
license: 
hidden: false
comments: true
readingTime: true
categories: ["樹莓派", "nodejs"]
tags: ["pi", "raspberry pi", "樹莓派", "溫度監控", "pi-pen", "node.js"]
draft: false
---

## 前言
這是我所設計的一個簡易網頁應用，我叫他 pi-pen  

他是使用 node.js 作為後端，利用樹莓派內建的系統溫度文件或系統命令來獲取溫度 

接著透過 javascript 將數據實時每 5 秒更新並顯示在網頁上。  

{{<link-card name="DEMO" desc="demo website" link="https://pi-pen.penli.quest" img="">}}

## 使用方式

### 程式碼結構 
**pi-pen**  
├ [server.js](https://github.com/penyt/pi-pen/blob/main/server.js)    
└ public  
&emsp; └ [index.html](https://github.com/penyt/pi-pen/blob/main/public/index.html)  

 {{<gist-title "server.js" penyt 95a0b903a236dcc9feac0b5c8663395a server.js>}}

 {{<gist-title "/public/index.html" penyt 95a0b903a236dcc9feac0b5c8663395a index.html>}}

### 實際操作
到你想要存放程式碼的位置，將我的 repo 複製下來 `git clone https://github.com/penyt/pi-pen.git`  

或是複製 `server.js` 和 `/public/index.html` 這兩個主要檔案也可以。  

接著在 pi-pen 資料夾下，執行

```
node server.js
```
若樹莓派沒有 node，先 [看這個](https://nodejs.org/zh-tw/download) 安裝  

此時，在瀏覽器打上 [http://localhost:5472](http://localhost:5472) 就可以看到如下的畫面，這就是樹莓派的溫度  

<img width="1120" alt="10min-bigscreen" src="https://github.com/user-attachments/assets/318254b7-477a-40db-9d22-3f013fb6c525" />
  
右邊有下拉選單可以視需求調整要觀察的時間  

### 關於資料儲存
這個程式主要只是在快速有圖形介面觀察溫度，而不需要監測硬體，主要目的在於方便快速  

所以我用來存放資料的 json 檔案（程式執行後會自動生成在 pi-pen 資料夾裡），只會儲存 **24小時** 的數據！！！  

超過 24 小時的會自動被刪除，不要把它當成永久儲存溫度數據的地方喔  


## 如何在背景運行？
我這邊使用 [PM2](https://pm2.io/docs/runtime/guide/installation/)，來達成背景運作的功效  

若樹莓派上沒有 PM2 那麼就用 npm 來安裝（官方的方式）
```
npm install pm2 -g
```

接著進入 `pi-pen` 資料夾（`cd /path/to/pi-pen`），執行：
```
pm2 start server.js
```
如果有更改 `server.js` 的檔名，這裡要記得改掉
  
查看 PM2 狀態：
```
pm2 status
```

查看日誌：
```
pm2 logs
```  
  
這時就算關閉終端，這個網頁應用還是會繼續運行  

## 也可使用 vcgencmd 命令來獲取溫度
只需要將 `server.js` 替換成底下這個 `server-vcgencmd.js` 就可以了 
 {{<gist-title "server-vcgencmd.js" penyt 95a0b903a236dcc9feac0b5c8663395a server-vcgencmd.js>}}

by Pen