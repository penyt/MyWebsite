---
title: "在 Docker Swarm 部署 traefik 以實現跨主機代理"
description: 跨主機反向代理
date: 2025-01-16T14:33:23Z
image: 
slug: docker-swarm-traefik
weight: -9
math: 
license: 
hidden: false
comments: true
readingTime: true
categories: ["docker","yml","traefik"]
tags: ["traefik", "docker", "docker swarm", "反向代理"]
draft: false
---

## 前言
如果你有多台主機，並且架設了 docker 服務，其實可以不用再每一台主機上都部署 traefik 來處理每個容器的反向代理和 TLS 證書  

當然，如果在同一個區域網路下，可以簡單地使用 traefik 的動態配置文件做到  

可是如果今天不在同一區域網路下呢？常見的情況就是在不同區域網路下的多台 VPS  

又或者，就是想要使用 docker 的 traefik label 來設定中間件（middleware）呢？  

畢竟如果想要添加 authentik、authelia 這類的授權頁面，或者使用 crowdsec，利用 traefik 的中間件非常方便設置  

其實還有一個方法，那就是在 docker swarm 下部署容器，並利用 overlay network 來做到跨越多台 docker 主機的作用，也就是這篇文章的內容  

## 🐧 docker swarm 的好處
### I. 需求少
如果你的機器沒有公共 IP（public IP），像是宿舍或公寓裡自架的伺服器，你未必會有最上游 NAT 的權限來設置端口轉發（port fowarding），那麼在這台主機上所設置的服務，便會無法暴露到外網  

而若使用 docker swarm，便只需要在一台機器上部署 traefik 進行反向代理，就可以把你家裡伺服器上的服務或網站暴露到外網  

**<center>好處就是：只需要有一個公共 IP、只需要部署一個 traefik</center>**  
<br>
且 docker swarm 是 docker 內建的功能，在完整安裝 docker engine 的時候就存在了，也不會佔用很多資源  

### II. 容易設置
使用 docker swarm 只需要簡單修改一下 docker 的 "labels" 區塊，和部署應用程式時的 docker 命令，就可以完成了  

## 🐧 操作方法
### 前提
<font color=#FF0000> host1 </font> :  「擁有」公共 IP，並且 traefik 將部署在這裡  
<font color=#3FB6F5> host2 </font> :  「沒有」公共 IP，其他的 app 會部署在這裡  
### 第一步，初始化 docker swarm
在 <font color=#FF0000> host1 </font> 執行:

```
docker swarm init
```
他會輸出像這樣的內容，但這是設置 worker 用的token：  
`docker swarm join --token THISIS-7-89456123789aworker456987token....... IP_ADDR:2377`  
  
另外還會有一行，這行才是我們需要使用的：  
`To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.`  

<br>

所以接著，繼續在 <font color=#FF0000> host1 </font> 執行:
```
docker swarm join-token manager
```
他會輸出像這樣的內容，這是設置 **manager** 用的token：  
`docker swarm join --token MAGENA-7-89456123789r456987token....... IP_ADDR:2377`  

<br>

### 第二步，將 host2 設置成 manager （非 worker，因為 worker 無法自行部署 app ）
切換到 <font color=#3FB6F5> host2 </font> 後，執行 **MANAGER** 的設置命令  
```
（記得複製你自己的）docker swarm join --token MAGENA-7-8943789r487token....... IP_ADDR:2377
```
接著，執行這個命令來檢查節點的狀態  
```
docker node ls
```

應該會輸出像這樣的內容:  
<blockquote>
ID                            &emsp;&emsp;&emsp;&emsp;
HOSTNAME                      &emsp;&emsp;
STATUS    &emsp;&emsp;
AVAILABILITY   &emsp;&emsp;&emsp;
MANAGER STATUS   &emsp;&emsp;&emsp;
ENGINE VERSION    
  
idofhostt2     &emsp;&emsp;
host2     &emsp;&emsp;&emsp;&emsp;
Ready     &emsp;&emsp;&emsp;
Active         &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
Reachable        &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
27.3.1
    
idofhostt1 *   &emsp;
host1   &emsp;&emsp;&emsp;&emsp;
Ready     &emsp;&emsp;&emsp;
Active         &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
Leader           &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
27.3.1  
</blockquote>

現在，我們就成功透過 docker swarm 來連接兩台跨網路的裝置了！  

<br>
<br>

### 第三步，新增 traefik 的 docker-compose.yml 和其他配置文件 [^1]
在 <font color=#FF0000> host1 </font> 新增如下方的資料夾和檔案  
* 新增資料夾 : `mkdir traefik`  
* 進入資料夾 : `cd traefik`  
* 新增檔案 : `touch traefik.yml config.yml acme.json docker-compose.yml`  
* 設置 acme 的權限 : `chmod 600 acme.json`
 
**traefik**  
├ docker-compose.yml  
├ traefik.yml  
├ config.yml  
└ acme.json  

編輯 [docker-compose.yml](https://github.com/penyt/traefik-docker-swarm/blob/main/traefik/docker-compose.yml) `nano docker-compose.yml`，把連結裡的內容貼上;  

編輯 [traefik.yml](https://github.com/penyt/traefik-docker-swarm/blob/main/traefik/traefik.yml) `nano traefik.yml`，把連結裡的內容貼上  

---

**(此步驟非必要，但強烈建議) 加上哈希密碼**
```
sudo apt-get install apache2-utils
```
將「username」和「yourpassword」設置成你自己的用戶名和密碼  
```
htpasswd -nb username yourpassword
```
命令執行後，會出現像這樣的一串：`username:$apr1$8VzK7EwL$4Z9T.HqxGkJpAqVnqp4Ol1`。  

把所有的 `$` 改成 `$$` (以避免 docker compose 會把單個`$`譯為環境變數)  

接著，把 docker-compose.yml 裡面的 `user:hashed_password` 改成剛剛出現的那一串    

<br>
<br>

### 第四步，部署 traefik
在 <font color=#FF0000> host1 </font> 執行：  
```
docker stack deploy -c docker-compose.yml traefik
```
或使用任何你喜歡的名稱 `docker stack deploy -c docker-compose.yml traefik_stack_name`  

<br>

在 <font color=#FF0000> host1 </font> 和 <font color=#3FB6F5> host2 </font> 檢查  
```
docker service ls
```
輸出如下 :   
<blockquote>
ID             &emsp;&emsp;&emsp;&emsp;&emsp;
NAME              &emsp;&emsp;&emsp;&emsp;&emsp;
MODE         &emsp;&emsp;
REPLICAS   &emsp;&emsp;
IMAGE                  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
PORTS  
  
rtafeiksid     &emsp;&emsp;
traefik_traefik   &emsp;&emsp;
replicated   &emsp;
1/1        &emsp;&emsp;&emsp;&emsp;
traefik:latest
</blockquote>

<br>
<br>

### 第五步，新增 whoami 的 docker-compose.yml
切換到 <font color=#3FB6F5> host2 </font> 新增如下方的資料夾和檔案   
* 新增資料夾 : `mkdir whoami`  
* 進入資料夾 : `cd whoami`  
* 新增檔案 : `touch docker-compose.yml`  
 
**whoami**  
└ docker-compose.yml  

編輯 [docker-compose.yml](https://github.com/penyt/traefik-docker-swarm/blob/main/whoami/docker-compose.yml) `nano docker-compose.yml`，把連結裡的內容貼上  

<br>
<br>

### 第六步，部署 apps (whoami)
在 <font color=#3FB6F5> host2 </font> 執行：  
```
docker stack deploy -c docker-compose.yml whoami
```
或使用任何你喜歡的名稱 `docker stack deploy -c docker-compose.yml whoami_stack_name`

<br>

在 <font color=#FF0000> host1 </font> 和 <font color=#3FB6F5> host2 </font> 檢查  
```
docker service ls
```
輸出如下 : 
<blockquote>
ID             &emsp;&emsp;&emsp;&emsp;&emsp;
NAME              &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
MODE         &emsp;&emsp;
REPLICAS   &emsp;&emsp;
IMAGE                  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
PORTS  

rtafeiksid     &emsp;&emsp;
traefik_traefik   &emsp;&emsp;&emsp;&emsp;
replicated   &emsp;
1/1        &emsp;&emsp;&emsp;&emsp;
traefik:latest  

whoidami   &emsp;&emsp;
whoami2_whoami2   &emsp;&emsp;
replicated   &emsp;
1/1        &emsp;&emsp;&emsp;&emsp;
traefik/whoami:v1.10   
</blockquote>

<br>
<br>

### 第七步，完成
現在，應該可以透過你在 traefik 標籤裡設置的域名來訪問 traefik 的 dashboard 或是 whoami 的頁面了

若有出現任何錯誤，記得:
* 確認你的 DNS 紀錄有將域名指向 <font color=#FF0000> host1 </font>
* 檢查 traefik 的日誌 `docker service logs traefik_traefik`
* 檢查 docker swarm 的 overlay network `docker network inspect proxy`

<br>
<br>
by Pen


[^1]:traefik deployment reference: [JimsGarage](https://github.com/JamesTurland/JimsGarage/blob/main/Traefik/docker-compose.yml)

