---
title: "Pihole 設置 Wildcard Dns"
description: pihole local DNS
date: 2024-07-07T15:30:40Z
image: 
slug: pi-wildcard-dns
weight: -5
math: 
license: 
hidden: false
comments: true
categories: ["DNS"]
tags: ["pihole","dns 加油"]
draft: false
---

## 介紹 Wildcard DNS
甚麼是 Wildcard DNS??  

要讓 ```whatever.hi.my.domain``` 、
 ```whenever.hi.my.domain``` 、 
 ```whoever.hi.my.domain``` 全部指向 54.87.87.87 IP 位址  
 
不用設置三個 DNS 紀錄，設置一個就好了  

讓所有 ```.hi.my.domain``` 的子域名全部指向同一個 54.87.87.87 IP 位址，這就是 Wildcard Dns  

## PiHole UI 設置出現錯誤
如果像是 bind 之類的 DNS，可能只要直接設置 A 紀錄：```*.hi.my.domain``` => 54.87.87.87，就可以了  

但是如果用 pihole 的 local DNS 的話，用 UI 介面是沒有辦法設置的，如果你像底下這樣輸入，就會顯示錯誤訊息  
![錯誤訊息](https://myrr.penli.quest/content/pihole-wildcard-dns/pidns.webp)  

但我們可以去修改 pihole 的配置文件就可以做到了  

## 更改 PiHole 配置文件

首先進入 dnsmasq.d 的資料夾，然後新建立一個 XX-wildcard.conf 的檔案  

```shell
cd /etc/dnsmasq.d/
sudo nano 05-wildcard.conf
```

在這個檔案輸入：
 {{<gist-title "/etc/dnsmasq.d/05-wildcard.conf" penyt ed7763430df29eb1475f398c75dd58c5 05-wildcard.conf>}}
之後保存退出 (記得把域名和 IP 改成你要的)  

然後讓 pihole 重新抓你的配置檔
```shell
service pihole-FTL restart
```

這樣其實就完成了，要確認的話用 nslookup，他就會顯示你設定的 IP 了 (當然你 nslookup 的環境要使用 pihole 的 dns，不然怎麼 nslookup 都看不到喔)

```shell
nslookup whatever.hi.my.domain
nslookup whenever.hi.my.domain
nslookup whoever.hi.my.domain
```

by Pen