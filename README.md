# docker-webtools
The lamp and vscode editor on docker.
##### Keyword: [ visual studio code ] [ VSCODE ] [ php ]  docker ] [ xdebug ] [ LAMP ]

## 主題: 集成 LAMP 與 vscode editor 開發環境於 docker

## 關聯圖
![Imgur](https://i.imgur.com/Hdz9dVT.png)
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
│   │{ creat database by your sqlfile or empty }
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
- X window: VcXsrv(Windows)、XQuartz(Mac)
- OS: Windows10( Hyper-V ) or Mac 都可

## 設定:

### 步驟1
1. 滑鼠右鍵工作列右邊的鯨魚小圖案 docker->settings->Shares Drives
* ![Imgur](https://i.imgur.com/KMActoG.png)
2. 選擇給 docker volumes 存放的硬碟
* ![Imgur](https://i.imgur.com/tHjnaBG.png)

### 步驟2
#### Windows 10
1. 安裝完 VcXsrv 後，直接點擊運行
2. 工作列右邊有 VcXsrv 小圖案及是啟動完成
* ![Imgur](https://i.imgur.com/KSJFpF9.png)

#### Mac
1. 安裝完 XQuartz 後，直接點擊運行

### 步驟3
1. 將 docker-webtools clone 到本機端，以VSCODE開啟 docker-xdebug 目錄資料夾
2. 在 ./docker-webtools/html/ 底下可放你入的專案，或著只是測試檔案都可

### 步驟4 (是 windows 的可跳過)
1. 點開編輯 ./docker-webtools/docker-compose.yml
2. 請依本機系統來選擇，預設是 Windows
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
* ![Imgur](https://imgur.com/9zd7qFx.png)
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
sudo apache2-foreground & #於後台啟動 apache
``` 
* ![Imgur](https://i.imgur.com/sly1No6.png)
3. 點擊左側臭蟲圖示之後顯示 debug 模式，再點擊左上邊的綠色箭頭，開始監聽~
4. 開啟本機上的瀏覽器，進入網頁 [http://localhost/](http://localhost/ "http://localhost/")
* ![Imgur](https://i.imgur.com/67uKHIq.png)
# 5. 開始除蟲啦~~~
* ![Imgur](https://i.imgur.com/S3qleX4.png)
# 備註
- vscode 開啟終端機快捷鍵  `` Ctrl + ` ``
- 虛擬CUI 開啟中文輸入法   ` Shift + space `
- 要停止運作的話，和步驟5一樣，但請輸入
#### 指令:
```shell
docker-compose stop #暫停運作，會保留容器
docker-compose down #結束運作，會刪除容器
``` 
- 開發環境資訊:
* ![Imgur](https://i.imgur.com/icEgJA4.png)

## Video
[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/4G4ZVbLVres/0.jpg)](https://www.youtube.com/watch?v=4G4ZVbLVres&feature=youtu.be)

## 參考
- [vscode-php-debug](https://github.com/felixfbecker/vscode-php-debug"https://github.com/felixfbecker/vscode-php-debug") 
- [setup-xdebug-on-phpstorm-and-visual-studio-code-using-docker-on-windows-hyper-v](https://medium.com/@sbuckpesch/setup-xdebug-on-phpstorm-and-visual-studio-code-using-docker-on-windows-hyper-v-9c385dd732c9"https://medium.com/@sbuckpesch/setup-xdebug-on-phpstorm-and-visual-studio-code-using-docker-on-windows-hyper-v-9c385dd732c9") 
- [docker-for-windows release-notes](https://docs.docker.com/docker-for-windows/release-notes/"https://docs.docker.com/docker-for-windows/release-notes/") 
- [jess/vscode](https://github.com/jessfraz/dockerfiles/tree/master/vscode"https://github.com/jessfraz/dockerfiles/tree/master/vscode")
-  [vscode getstarted keybindings](https://code.visualstudio.com/docs/getstarted/keybindings"https://code.visualstudio.com/docs/getstarted/keybindings")
- [docker hub php](https://hub.docker.com/_/php/"https://hub.docker.com/_/php/")
