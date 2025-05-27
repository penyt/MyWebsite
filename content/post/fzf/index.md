---
title: "fzf -- fuzzy finder 模糊搜尋器"
description: fzf
date: 2025-04-13T00:30:06+08:00
image: 
slug: /fzf
weight: -10
math: 
license: 
hidden: true
comments: true
readingTime: true
categories: [""]
tags: ["fzf","fuzzy finder"]
draft: false
---
## 🐧 fzf 介紹
fzf，全名 fuzzy finder，意思就是模糊搜尋工具

{{< link-card name="fzf 模糊搜尋工具" desc="fuzzy finder" link="https://github.com/junegunn/fzf" img="https://raw.githubusercontent.com/junegunn/i/master/fzf.png" >}}

可以搜尋檔案、資料夾、運行程序等等，且只要輸入部分，就會顯示出所有結果，正所謂「模糊」搜尋。  

上面有附上 github 連結，覺得好用的話可以給開發者一個星星。  

## 🐧 fzf 安裝
### brew
用 brew 安裝
```
brew install fzf
fzf --version
```
設定會幫你把該有的路徑加進 .bashrc 或 .zshrc 檔案，之後 source 重整：
```
$(brew --prefix)/opt/fzf/install
source ~/.zshrc
```

### apt
```
sudo apt install fzf
fzf --version
```
加入 .bashrc
```
eval "$(fzf --bash)"
source ~/.bashrc
```

## 🐧 fzf 實用功能
### 搜尋檔案
```
fzf
```
### 搜尋歷史命令
快捷鍵： `Ctrl + R`

### 預覽檔案內容
```
fzf --preview 'bat --style=numbers --color=always {}'
```
`--preview`：會在 fzf 搜尋列表的旁邊有一個預覽視窗  
`{ }` 裡面會被替換成目前選中的檔案路徑  

- bat 的參數：
  - `--style`：我這裡用 numbers 顯示行數  
  - `--color`：顯示語法顏色  

可以去 github 文檔看看更多的設定方式  
  



### 預覽檔案內容 Enter 進入編輯器
```
fzf --preview 'bat --style=numbers --color=always {}' | xargs -n 1 nvim
```
後面的 nvim 可以改成 vim 或 nano，或任何喜歡的編輯器  

用 `xargs` 把選到的檔案傳給 nvim（編輯器） 開啟  

### 砍掉運行程式
