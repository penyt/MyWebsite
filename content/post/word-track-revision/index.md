---
title: "Word 追蹤修訂一次改為實際文字顏色"
description: 我是不想一個一個改顏色、底線、刪除線啦
date: 2024-10-04T13:37:54Z
image: 
slug: word-track-revision
weight: -6
math: 
license: 
hidden: false
comments: true
categories: ["word"]
tags: ["word","追蹤修訂"]
draft: false
---
最近用 word 的追蹤修訂在改文件，但是要給別人的時候，別人卻說不會用追蹤修訂，也不想學...  

我是不想一個一個標註顏色、底線、刪除線啦 🙃🙃🙃  

所以寫了這個 VBA 一次解決  

## 情況
情況分成兩種：  
&emsp;&emsp;第一種：只把插入的標註紅色+底線  
&emsp;&emsp;第二種：插入的部分變紅色文字+底線，刪除的變成藍色文字+刪除線  

## 邏輯
邏輯上來說很簡單：  
&emsp;&emsp;1.偵測修改的部分  
&emsp;&emsp;2.插入的部分直接先加上顏色和底線  
&emsp;&emsp;3.接受所有插入部分  
&emsp;&emsp;4.刪除的部分複製一份放在原本的詞句後面（因為刪除的實際上來說已經是不存在的了，只是因為追蹤修訂暫時保留）  
&emsp;&emsp;5.在複製的那一份加上顏色和刪除線  
&emsp;&emsp;**6.接受所有刪除部分**  
  
最後一步稍微思考一下其實就不會弄錯  

## 在 word 操作
&emsp;&emsp;第一步：打開 word，按 ```Alt``` + ```F11```  
&emsp;&emsp;第二步：工具列 「插入」+「模組」 （insert + module）  
&emsp;&emsp;第三步：底下這些程式碼挑適合的貼上  
&emsp;&emsp;&emsp;&emsp;&emsp;（如果要儲存，右鍵這個專案，「匯出檔案」）  
&emsp;&emsp;第四步：關閉 VBA 視窗  
&emsp;&emsp;第五步：在 word，按 ```Alt``` + ```F8```  
&emsp;&emsp;第六步：選擇 InsertRed 或 InsertRedDeleteBlue 或任何自己取的名字，點「執行」  
&emsp;&emsp;第七步：等他（追蹤修訂的數量決定需要的時間），完成  

基本上是不會有錯誤啦，但是也可以人工再檢查一下  

{{<gist-title "插入是紅色，刪除是藍色" penyt 344101686d5ef5f4b13e62fb073e792d redblue.bas>}}
{{<gist-title "只有插入是紅色" penyt 344101686d5ef5f4b13e62fb073e792d red.bas>}}

by Pen