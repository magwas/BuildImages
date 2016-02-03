FROM ubuntu:trusty

RUN locale-gen en_US en_US.UTF-8 && \
    dpkg-reconfigure locales

RUN apt-get update

RUN apt-get -o Dpkg::Options::="--force-confold" --force-yes -fuy upgrade

RUN apt-get -y install software-properties-common
RUN add-apt-repository ppa:openjdk-r/ppa -y
RUN apt-get update
RUN apt-get -y install openjdk-8-jdk maven git firefox xvfb libwebkitgtk-3.0-0

RUN git clone https://github.com/magwas/zenta.git
RUN cd zenta;mkdir ./org.rulez.magwas.zenta.integrationtests/generated-images;Xvfb :99&export DISPLAY=:99;mvn install;echo oktofail


