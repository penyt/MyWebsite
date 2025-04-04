---
title: "Hugo Stack 主題 -- 目錄 toc 設置更改"
description: toc toc toc
date: 2024-03-23T16:15:07Z
image: 
slug: stack-toc
weight: -4
math: 
license: 
hidden: false
comments: true
categories: ["hugo","hugo stack"]
tags: ["toc太需要了吧","改改改"]
draft: false
---

## 用戶體驗
stack主題的toc在手機端並不顯示，但我自己覺得目錄蠻重要的  

而且手機寬度縮短，長度就會變長，沒有目錄的話就要一直滑滑滑，體驗並不是很好  

可是電腦版目錄在右側的版面我也很喜歡，所以就打算改成 手機端目錄在文章開頭、電腦端則照舊在右側  

## 參考、引入 toc 的 html
之前 [github issue](https://github.com/CaiJimmy/hugo-theme-stack/issues/422) 有人做過把目錄放進文章，根據這個改了一下，加上手機端的條件、更改樣式 (程式碼在後面)  

在想要目錄出現的位置引入 toc-inline 就可以了（我放在 content.html 的```<section class="article-content">```裡面第一行  

{{<gist-title "/layouts/partials/article/components/content.html" penyt ded9d1f55c1f10673a35ea4e00117394 content.html>}}

## 程式碼

{{<gist-title "/layouts/partials/article/components/toc-inline.html" penyt ded9d1f55c1f10673a35ea4e00117394 toc-inline.html>}}

{{<gist-title "/assets/scss/custom.scss" penyt ded9d1f55c1f10673a35ea4e00117394 toc-custom.scss>}}
  
## 效果
![手機端效果1](http://ncloud.penli.quest/s/toc-preview/download/preview.png)
<center>🔼 手機端效果1</center>  
  
---

![手機端效果2](http://ncloud.penli.quest/s/preview2/download/preview2.png)
<center>🔼 手機端效果2</center>  
  
<br>
  
 by Pen
