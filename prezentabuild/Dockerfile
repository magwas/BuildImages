FROM ubuntu:trusty

RUN locale-gen en_US en_US.UTF-8 && \
    dpkg-reconfigure locales

RUN apt-get update

RUN apt-get -o Dpkg::Options::="--force-confold" --force-yes -fuy upgrade

RUN apt-get -y install software-properties-common
RUN add-apt-repository ppa:openjdk-r/ppa -y
RUN apt-get update
RUN apt-get -y install openjdk-8-jdk maven git firefox libwebkitgtk-3.0-0 tightvncserver

