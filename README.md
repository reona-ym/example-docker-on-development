# example-docker-on-development

## これ is 何
docker上で開発環境を作成するサンプル  
`docker-compose up`でssh接続可能な開発環境を作成する  
`src`フォルダはdocker内の`/usr/local/src`とリンクする  

## 下記のインストールを行う  
* [Docker Desktop for Windows][docker-desktop-for-win]  
* [Git for Windows][git-for-win]  
* [vscode][vscode]

## VSCodeの起動
vscodeを起動する

## エクステンションの追加
ctrl + shift + x ボタンを押下し、エクステンションの追加を開く  
以下のエクステンションを追加する  
* Docker (発行者：Microsoft)
* Remote Development (発行者：Microsoft)

## Gitリポジトリのclone
適当なフォルダを作成し、pullする

## 仮想環境の起動
Dockercompose.ymlを右クリックし、Dockercompose upを選択

## ssh接続を行う

``` sample
Host tomcat
  HostName localhost
  User user
  Port 40022
  IdentityFile C:\work\key\id_rsa
  LocalForward 48080 localhost:8080
```

[docker-desktop-for-win]:https://hub.docker.com/editions/community/docker-ce-desktop-windows
[git-for-win]:https://gitforwindows.org/
[vscode]:https://azure.microsoft.com/ja-jp/products/visual-studio-code/?cdn=disable