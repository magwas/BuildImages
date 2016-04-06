FROM ubuntu:trusty

RUN locale-gen en_US en_US.UTF-8 && \
    dpkg-reconfigure locales

RUN apt-get update

RUN apt-get -o Dpkg::Options::="--force-confold" --force-yes -fuy upgrade

RUN apt-get update
RUN apt-get -y install openjdk-7-jre wget git xvfb unzip docbook-xsl make firefox libwebkitgtk-1.0-0 tightvncserver dblatex

RUN useradd develop
RUN mkdir /home/develop
RUN chown develop:develop /home/develop

RUN mkdir /home/develop/lib; cd /home/develop/lib ; wget -q http://downloads.sourceforge.net/project/saxon/Saxon-HE/9.4/SaxonHE9-4-0-2J.zip;unzip SaxonHE9-4-0-2J.zip ; rm -f SaxonHE9-4-0-2J.zip

RUN mkdir /home/develop/build;cd /home/develop/build; git clone https://github.com/expath/xspec.git

RUN mkdir /opt;cd /opt ; wget -q http://downloads.sourceforge.net/project/zenta/Zenta-linux-x86_64.zip ; unzip Zenta-linux-x86_64.zip ; rm -f Zenta-linux-x86_64.zip

RUN ln -s /home/develop/lib /root/lib

RUN ln -s /home/develop/build /root/build

CMD sudo su - develop



