FROM ubuntu:16.04
LABEL maintainer="Hideyuki Sawahata <sawahata@uec.ac.jp>"
# プロキシ情報受け取り
ARG HTTP_PROXY
ARG HTTPS_PROXY
# インストール時にタイムゾーンを聞かれるとimageのビルドが止まってしまうのでこれで回避
ENV DEBIAN_FRONTEND noninteractive
ENV http_proxy=${HTTP_PROXY}
ENV https_proxy=${HTTPS_PROXY}
# ディスプレイサーバーのディスプレイ番号を指定
# リモートでコンテナを起動するならともかく、ホストPCで動かすならこれで恐らく動くはず
# 大貧民サーバーをビルドするのに(恐らく)必要なパッケージ群
# フォント関連はホスト側で補完するため、コンテナ側には必要ないが、
# ビルド時にチェックされるらしいので入れる
RUN apt-get -y update && \
    apt-get -y install xorg-dev x11-xserver-utils x11-utils\
    fontconfig \
    dbus-x11 \ 
    libxaw7-dev \
    gcc make \
    gsfonts-x11 \
    autoconf automake libtool

RUN fc-cache -fv
# サーバーに必要なファイル群をコピー
COPY ./server /

## サーバービルド
RUN ./configure && make
# コンテナ作成時に実行するためのコマンド
WORKDIR /src
ENV DISPLAY=:1
CMD [ "./tndhms" ]
