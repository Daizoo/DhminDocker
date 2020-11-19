# DhminDocker

## 概要

DhminDocker(大貧民Docker)とはコンピューター大貧民をDockerを用いて各種OSにおいて実行・開発を容易にするためのシステムです。
動作確認済みOSは以下の通りです。

- Windows10
- Ubuntu20.04 LTS

## 準備

基本的には

- **Docker**
- **docker-compose**

の2つが必要です。[公式ドキュメント](https://docs.docker.com/get-docker/)より使用しているOSに合わせて、インストールを行ってください。インストール後に下記より、各種OS別に必要なソフトウェアをインストールするようにしてください。

### 各種OS別準備

#### Windows10

[VcXsrv](https://sourceforge.net/projects/vcxsrv/)をインストールしてください。インストール後にはVcXsrvを起動してください。

#### Mac
`brew`を用いてXquartzを導入してください。

```bash
brew install xquartz
```

Xquartz起動後に、環境設定から「ネットワーク・クライアントからの接続を許可」にチェックをしてください。
チェックした後に、次のコマンドを実行してください。
```bash
echo "HOST=$HOST" >> ~/.bashrc
source ~/.bashrc
```

#### Ubuntu

次のコマンドを入力してください。

```bash
sudo apt install gsfonts-x11 -y
fc-cache -fv
```

実行後はログインしなおしてください。

## インストール

### to Windows10

上記のbranchタブを`Windows`に切り替えてダウンロードする、もしくは次のコマンドをPowershellに入力して、ダウンロードしてください(**要: [Git for Windows](https://gitforwindows.org/)**)。

```bash
git clone https://github.com/Daizoo/DhminDocker
git archive windows --output=dhmin_docker.zip
```

### to Ubuntu

上記のbranchタブを`Ubuntu`に切り替えてダウンロードする、もしくは次のコマンドを実行して、ダウンロードしてください。

```bash
git clone https://github.com/Daizoo/DhminDocker
git archive ubuntu --output=dhmin_docker.zip
```

## 実行方法

次のコマンドを実行することでサーバー及び、クライアントのビルドと実行をすることが可能です。

```shell
docker-compose up --build
```

ただし、Ubuntuでは実行前に必ずこのコマンドを実行してください。
```bash
xhost local:
```

## クライアント開発

### 開発
### クライアントイメージ構築用のDockerfile

クライアントを構築するためのDockerfileは`client/default`に含まれている`Dockerfile`、及び
[公式のドキュメント](https://docs.docker.jp/engine/userguide/eng-image/dockerfile_best-practices.html)を参考にして記述してください。最低でも**コンテナ作成時に大貧民サーバー側に接続するように設定してください。**
例えば以下のように記述してください。
```dockerfile
# 開発に使うOSの設定
FROM ubuntu 
# 必要な環境変数(プロキシの設定など)を設定
ENV DEBIAN_FRONTEND noninteractive
# コンパイルに必要なファイルのインストールなど
RUN apt-get update && apt-get install -y gcc make
# ソースコードのコピー
COPY ./src /
# クライアントのビルド
RUN ./configure && make
# コンテナ作成時に実行するコマンド
CMD ./client
```
### 対戦クライアントの設定

対戦させるクライアントは同梱されている`docker-compose.yml`より設定してください。
プレイヤー1を変更する場合にはファイル内の`context`を**Dockerfileとクライアントのプログラムソースが同梱されているフォルダを指定するようにしてください**。
```yaml
  dhmin_player1:
    image: dhmin/player1
    build: 
      context: ./client/default # 使用するクライアントプログラムのあるフォルダを指定
      args: # ビルドに使用する環境変数とかを指定
        HTTP_PROXY: $http_proxy
        HTTPS_PROXY: $http_proxy
    container_name: player1
    depends_on: 
      - dhmin_server
    networks: 
      - dhmin-networks
```