---
title: "Hugo Stack 主題 -- 我的 Shortcode"
description: "放我的 shortcode 和自己的筆記們"
date: 2024-04-02T09:25:58Z
image: 
slug: my-Shortcode
weight: 999
math: 
license: 
hidden: false
comments: true
categories: ["置底","hugo","hugo stack"]
tags: ["好難","自己的筆記也放","不然自己每次都要查"]
draft: false
---
## 📏 文字對齊
<center> &lt;center&gt; 置中了吧 &lt;/center&gt; </center>
<center> 置中了吧 </center>
  
<p align="left"> &lt;p align="left"&gt; 靠左了吧 &lt;/p&gt; <br> 靠左了吧 </p>
  
<p align="right"> &lt;p align="right"&gt; 靠右了吧 &lt;/p&gt; <br> 靠右了吧 </p>
  
---


## 🎨 文字顏色
```<font color=#FF0000> 紅色的文字 </font>```  
<font color=#FF0000> 紅色的文字 </font>
  
---


## 🌀 html 跳脫字元

|  字元   | 編碼  |
|  ----  | ----  |
| <  | 	```&lt;``` |
| >  |  ```&gt;``` |
| {  |  ```&#123;``` |
| }  |  ```&#125;``` |
|大空格|  ```&emsp;``` |
|小空格|  ```&nbsp;``` |


[html 字元](https://www.freeformatter.com/html-entities.html)
  
---



## 🔖 書籤連結
手機端會太擁擠，所以我讓圖片在手機端隱藏，img的地方加上```class="hide-on-mobile"```，```class="hide-on-mobile"``` 寫在 style 裡面  

> <div style="text-align:center">md 引入格式：&ensp; &#123;&#123;&lt;link-card name="" desc="" link="" img=""&gt;&#125;&#125;</div>  

- shortcode html 程式碼：  
https://gist.github.com/penyt/6e19f98476ffa5d737d8f8b268dcd0fa#file-link-card-html  
- 樣式 scss 程式碼：  
https://gist.github.com/penyt/6e19f98476ffa5d737d8f8b268dcd0fa#file-link-card-var-scss  

### 效果 (書籤連結)
"shortcode html" 連結做成書籤：

{{< link-card name="link-card.html" desc="shortcode html" link="https://gist.github.com/penyt/6e19f98476ffa5d737d8f8b268dcd0fa#file-link-card-html" img="http://ncloud.penli.quest/s/img/download/img.png" >}}

"樣式 scss" 連結做成書籤：

{{< link-card name="link-card-var.scss" desc="樣式 scss" link="https://gist.github.com/penyt/6e19f98476ffa5d737d8f8b268dcd0fa#file-link-card-var-scss" img="http://ncloud.penli.quest/s/img/download/img.png" >}}
  
---


## 👩🏻‍💻 gist 嵌入加上標題
原始的 gist 嵌入的樣式不喜歡，還有最底下那條 "hosted with ❤️ by GitHub"，我是選擇用自己的樣式保留著，畢竟是 github 的服務，直接刪掉總感覺不太恰當  

再用一個方框加上標題，可以自訂和gist檔案名稱不同的標題，標題也可以連結到gist的頁面  

penyt 是我的 github 帳號，你要換成自己的  

> <div style="text-align:center">md 引入格式：&ensp; &#123;&#123;&lt;gist-title "title" penyt ID (檔名)&gt;&#125;&#125;</div>

- shortcode html 程式碼：  
https://gist.github.com/penyt/6e19f98476ffa5d737d8f8b268dcd0fa#file-gist-title-html  
- 樣式 scss 程式碼：  
https://gist.github.com/penyt/6e19f98476ffa5d737d8f8b268dcd0fa#file-gist-scss  

### 效果 (gist 標題)
{{<gist-title "gist-title.html" penyt 6e19f98476ffa5d737d8f8b268dcd0fa gist-title.html>}}
{{<gist-title "gist.scss" penyt 6e19f98476ffa5d737d8f8b268dcd0fa gist.scss>}}
  
---


## 🔐文章加密

- shortcode html：  

- 樣式 scss：  

### 效果 (文章加密)
