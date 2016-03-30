FROM ubuntu:trusty

RUN locale-gen en_US en_US.UTF-8 && \
    dpkg-reconfigure locales

RUN apt-get update

RUN apt-get -o Dpkg::Options::="--force-confold" --force-yes -fuy upgrade

RUN apt-get -y install python make python-pip openjdk-7-jre\
    curl docbook-xsl apache2 libapache2-mod-wsgi swig libffi-dev\
    libnss3-tools unzip\
    git python-dev libssl-dev python-m2crypto xvfb

RUN apt-get install -y postgresql python-psycopg2
RUN service postgresql start ; sudo -u postgres createuser root ; sudo -u postgres createdb root -O root

RUN apt-get -y install python-pykcs11 libsofthsm python-coverage


ADD pdoauthtest /usr/local/bin/pdoauthtest
ADD requirements.txt /tmp/requirements.txt

RUN apt-get install -y wget
RUN pip install -r /tmp/requirements.txt

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

RUN apt-get -y install software-properties-common

ENV CHROME_DRIVER_VERSION 2.21

#===============
# Google Chrome
#===============
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
  && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update -qqy \
  && apt-get -qqy install google-chrome-stable \
  && rm /etc/apt/sources.list.d/google-chrome.list \
  && rm -rf /var/lib/apt/lists/*

#==================
# Chrome webdriver
#==================

RUN wget --no-verbose -O /tmp/chromedriver_linux64.zip http://chromedriver.storage.googleapis.com/$CHROME_DRIVER_VERSION/chromedriver_linux64.zip \
  && rm -rf /opt/selenium/chromedriver \
  && unzip /tmp/chromedriver_linux64.zip -d /opt/selenium \
  && rm /tmp/chromedriver_linux64.zip \
  && mv /opt/selenium/chromedriver /opt/selenium/chromedriver-$CHROME_DRIVER_VERSION \
  && chmod 755 /opt/selenium/chromedriver-$CHROME_DRIVER_VERSION \
  && ln -fs /opt/selenium/chromedriver-$CHROME_DRIVER_VERSION /usr/bin/chromedriver


ADD certs.json /etc/chromium-browser/policies/managed/
ADD certs.json /etc/opt/chrome/policies/managed/

#for debugging
RUN apt-get update
RUN apt-get -y install vnc4server opensc
RUN sed -i 's/peer/trust/' /etc/postgresql/9.3/main/pg_hba.conf
RUN cd /usr/local/lib ; wget -q http://downloads.sourceforge.net/project/saxon/Saxon-HE/9.4/SaxonHE9-4-0-2J.zip;unzip SaxonHE9-4-0-2J.zip ; rm -f SaxonHE9-4-0-2J.zip
RUN mkdir -p /dependencies/javascript
RUN curl http://code.jquery.com/qunit/qunit-1.18.0.js -o /dependencies/javascript/qunit-1.18.0.js
RUN curl http://code.jquery.com/qunit/qunit-1.18.0.css -o /dependencies/javascript/qunit-1.18.0.css
RUN curl https://raw.githubusercontent.com/JamesMGreene/qunit-reporter-junit/master/qunit-reporter-junit.js -o /dependencies/javascript/qunit-reporter-junit.js
RUN curl https://raw.githubusercontent.com/alex-seville/blanket/89266afe70ea733592f5d51f213657d98e19fc0a/dist/qunit/blanket.js -o /dependencies/javascript/blanket.min.js
RUN curl https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css -o /dependencies/javascript/bootstrap.min.css
RUN curl https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js -o /dependencies/javascript/bootstrap.min.js
RUN curl https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js -o /dependencies/javascript/jquery.min.js

CMD /bin/bash


