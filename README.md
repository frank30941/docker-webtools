# docker-webtools
The lamp and vscode editor on docker.
##### Keyword: [ visual studio code ] [ VSCODE ] [ php ]  docker ] [ xdebug ] [ LAMP ]

## 主題: 集成 LAMP 與 vscode editor 開發環境於 docker

## 關聯圖
![](https://i.imgur.com/Hdz9dVT.png)
<!-- ```mermaid
graph TD
X[main OS] -- X Window --- A(Container: apache, php, xdebug, vscode)
A --- C(Container:mariadb, HOST:mariadb)
B(Container:phpmyadmin) --- C
X -- http://localhost:80 --- A
X -- http://localhost:8080 --- B
```-->

## 資料夾結構
```
/docker-webtools
│README.md
│docker-compose.yml
|
└───/db
│   │{ creat databace by your sqlfile or empty }
│   
└───/html
|   │{your php project}
|   └───/.vscode
|       |{configfile}
|
└───/phpmyadmin
	└───/session
    	| {tmpfile}
        |
```

## 準備工具:
- docker: >=17.09.1( Docker for Windows OR Docker for Mac )
- docker images: frank30941/docker-webtools:latest、mariadb:latest、phpmyadmin/phpmyadmin:latest
- X window: VcXsrv
- OS: Windows10( Hyper-V ) or Mac 都可

## 設定:

### 步驟1
1. 滑鼠右鍵工作列右邊的鯨魚小圖案 docker->settings->Shares Drives
2. 選擇給 docker volumes 存放的硬碟

### 步驟2
1. 安裝完 VcXsrv 後，直接點擊運行
2. 工作列右邊有VcXsrv小圖案及是啟動完成

### 步驟3
1. 將 docker-webtools clone 到本機端，以VSCODE開啟 docker-xdebug 目錄資料夾
2. 在 ./docker-webtools/html/ 底下可放你入的專案，或著只是測試檔案都可

### 步驟4 (是 windows 的可跳過)
1. 點開編輯 ./docker-webtools/docker-compose.yml
2. <span style="color:red"> **重點在這!!** **重點在這!!** **重點在這!!** **很重要所以說三次!!** </span> 請依你的本機系統來選擇，預設是 Windows
```yml
  xdebug:
    image: frank30941/webtools:latest
    ports:
      - "80:80"
    restart: always
    environment:
      DISPLAY: docker.for.win.localhost:0.0
#      DISPLAY: docker.for.mac.localhost:0
```

### 步驟5
1. 開啟終端機
- Windows: cmd or PowerShell
- Mac: Terminal

#### 指令:
```shell
cd /***/***/***/docker-webtools/
docker-compose up -d
```
第一次會先 pull frank30941/docker-webtools:latest 的 image，會花很多時間，這時候就來沖杯咖啡吧~

### 步驟6
1. 建立完成並且容器開始運作後，會直接開啟容器內的vscode
2. 於 vscode 內開啟終端機，並輸入

#### 指令:
```shell
sudo apache2-foreground & //於後台啟動 apache
``` 
3. 點擊左上邊的綠色箭頭，開始監聽~
4. 開啟本機上的瀏覽器，進入網頁 [http://localhost/](http://localhost/ "http://localhost/")

# 5. 開始除蟲啦~~~
 
# 備註
- vscode 開啟終端機快捷鍵  `` Ctrl + ` ``
- 虛擬CUI 開啟中文輸入法   ` Shift + space `

## 參考
- [https://github.com/felixfbecker/vscode-php-debug](https://github.com/felixfbecker/vscode-php-debug"https://github.com/felixfbecker/vscode-php-debug") 
- [https://medium.com/@sbuckpesch/setup-xdebug-on-phpstorm-and-visual-studio-code-using-docker-on-windows-hyper-v-9c385dd732c9](https://medium.com/@sbuckpesch/setup-xdebug-on-phpstorm-and-visual-studio-code-using-docker-on-windows-hyper-v-9c385dd732c9"https://medium.com/@sbuckpesch/setup-xdebug-on-phpstorm-and-visual-studio-code-using-docker-on-windows-hyper-v-9c385dd732c9") 
- [https://docs.docker.com/docker-for-windows/release-notes/](https://docs.docker.com/docker-for-windows/release-notes/"https://docs.docker.com/docker-for-windows/release-notes/") 
- [https://github.com/jessfraz/dockerfiles/tree/master/vscode](https://github.com/jessfraz/dockerfiles/tree/master/vscode"https://github.com/jessfraz/dockerfiles/tree/master/vscode")
-  [https://code.visualstudio.com/docs/getstarted/keybindings](https://code.visualstudio.com/docs/getstarted/keybindings"https://code.visualstudio.com/docs/getstarted/keybindings")
