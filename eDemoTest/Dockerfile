FROM ubuntu:trusty

RUN locale-gen en_US en_US.UTF-8 && \
    dpkg-reconfigure locales

RUN apt-get update

RUN apt-get -o Dpkg::Options::="--force-confold" --force-yes -fuy upgrade

RUN apt-get -y install python make python-pip openjdk-7-jre\
    curl docbook-xsl apache2 libapache2-mod-wsgi swig libffi-dev\
    libnss3-tools unzip\
    git python-dev libssl-dev python-m2crypto xvfb

RUN apt-get -y install python-pykcs11 libsofthsm

ADD certs.json /etc/chromium-browser/policies/managed/

ADD pdoauthtest /usr/local/bin/pdoauthtest
ADD requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt


#firefox and chrome install code is from https://codeship.com/documentation/docker/browser-testing/
RUN apt-get -y install firefox

RUN apt-get -y install wget
# Setting the Firefox version and installation directory through environment variables.
ENV FIREFOX_VERSION 40.0
ENV FIREFOX_DIR /firefox
ENV FIREFOX_FILENAME $FIREFOX_DIR/firefox.tar.bz2

# Create the Firefox directory, download the custom Firefox version from Mozilla and untar it.
RUN mkdir $FIREFOX_DIR
RUN wget -q --continue --output-document $FIREFOX_FILENAME "https://ftp.mozilla.org/pub/mozilla.org/firefox/releases/${FIREFOX_VERSION}/linux-x86_64/en-US/firefox-${FIREFOX_VERSION}.tar.bz2"
RUN tar -xaf "$FIREFOX_FILENAME" --strip-components=1 --directory "$FIREFOX_DIR"

# Setting the PATH so our customer Firefox version will be used first
ENV PATH $FIREFOX_DIR:$PATH

# Set up the Chrome PPA
#RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
#RUN echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list
#
#RUN apt-get update
#
#RUN apt-get -y install google-chrome-stable
#
## Set up Chromedriver Environment variables
#ENV CHROMEDRIVER_VERSION 2.19
#ENV CHROMEDRIVER_DIR /chromedriver
#RUN mkdir $CHROMEDRIVER_DIR
#
## Download and install Chromedriver
#RUN wget -q --continue -P $CHROMEDRIVER_DIR "http://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip"
#RUN unzip $CHROMEDRIVER_DIR/chromedriver* -d $CHROMEDRIVER_DIR
#
## Put Chromedriver into the PATH
#ENV PATH $CHROMEDRIVER_DIR:$PATH

RUN apt-get -y install chromium-chromedriver

#for debugging
RUN apt-get -y install tightvncserver
CMD /bin/bash

