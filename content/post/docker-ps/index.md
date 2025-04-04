---
title: "docker ps 格式"
description: 調整 docker ps 輸出格式
date: 2024-03-19T08:52:50Z
math: 
license: 
hidden: false
comments: true
slug: docker-ps
weight: -1
categories: ["docker"]
tags: ["readable 可讀性高的"]
draft: false
---

## 問題
```docker ps``` 指令的輸出格式會很容易超出螢幕，不好閱讀 (這大家應該都知道不需要展示圖片了吧ㄎ)  

## 官方文件
docker有給出 ```--format``` 的 [官方文件](https://docs.docker.com/config/formatting/)  

## 自訂命令
這是我自己使用的調整格式後的docker ps命令  
{{< gist-title "command" penyt 7f5365b3be2baf76835bf6831d116b9c dockerps.sh >}}

## 輸出結果
輸出結果是這樣的：  
![輸出結果](https://myrr.penli.quest/docker-ps-img%2Fdockerps.webp)  
明顯好閱讀很多很多很多  

## Windterm快捷（我個人沒有再使用了）
> 我個人從去年後半開始便沒有再使用 windterm 了，原因不外乎 windterm 一直以來未完全開源的問題，我現在就是完全使用 vscode 和終端機了  

這邊我使用windterm快捷命令可以直接這樣輸入到快捷命令的Text:框框，也供大家參考：
{{< gist-title "windterm shortcut" penyt 7f5365b3be2baf76835bf6831d116b9c dockerpswt.sh>}}

<br>
<br>
如果空格在命令中無法使用，可以改成  \t  用跳脫字元，但我用GCP網頁的SSH叫都沒問題空格應該是可以使用的  

大概就這樣  

by Pen

