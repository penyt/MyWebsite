---
title: "關於用 docker 安裝 hugo 的 baseurl 問題"
description: baseurl 一直是 localhost:port
date: 2024-04-01T09:53:20Z
image: 
slug: hugo-docker-baseurl
weight: -3
math: 
license: 
hidden: false
comments: true
categories: ["docker","yml","hugo"]
tags: ["baseurl","不要再localhost了"]
draft: false
---

## 問題
用docker架設hugo網站的人可能不多，不過寫下前陣子遇到的問題  
  
在config檔案明明設置了baseurl，但是實際的permalink卻一直是https://localhost:1313  
  
很多需要重新導向的服務都會出問題（前陣子遇到utterance在登入github的時候導不回來  
  
後來爬了很多文發現是hugo server的問題，可能被其他預設的配置蓋掉了config的設定  
  
## 解決
當在容器裡進行hugo server或hugo的時候，直接指定```--baseurl```  

{{< gist-title "docker-compose.yaml" penyt 9917d11779859124057ff586134852ae >}}
  
我是用docker compose安裝hugo，所以直接加在command的地方，然後不要port  
  
設置好新的yaml後，再到yaml的資料夾裡面```docker compose up -d```  

permalink就變成https://my.domain.com了  
  
可以去網頁直接F12確認  
  
這時候utterance或其他重新導向的服務就可以正常使用不會導去localhost了  
  
## 參考資料
- https://github.com/JamesTurland/JimsGarage
- https://www.sujaypillai.dev/2020/08/2020-08-17-setting-up-blog-on-aws-using-traefik-docker/
  
by Pen