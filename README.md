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

## 使い方

次のコマンドを実行することでサーバー及び、クライアントのビルドと実行をすることが可能です。

```shell
docker-compose up --build
```

## クライアント開発