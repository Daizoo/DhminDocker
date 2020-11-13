FROM ubuntu:16.04
LABEL maintainer="Hideyuki Sawahata <sawahata@uec.ac.jp>"

ARG HTTP_PROXY
ARG HTTPS_PROXY

ENV DEBIAN_FRONTEND noninteractive
ENV http_proxy=${HTTP_PROXY}
ENV https_proxy=${HTTPS_PROXY}

RUN mkdir client
COPY ./src/* /client/
COPY ./wait-for-it.sh /client/
RUN apt-get update -y && apt-get install -y gcc make
WORKDIR /client
RUN ./configure && make
CMD sleep 10; ./client -h server -p 42485