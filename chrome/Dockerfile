FROM ubuntu:16.04

RUN echo "deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse\n \
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse\n \
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse\n \
deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse\n \
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse\n \
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse\n \
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse\n \
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse\n \
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse\n \
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse" >/etc/apt/sources.list

RUN apt-get update && apt-get install -y -q wget unzip dpkg libnss3-1d curl make sudo vim
RUN wget --no-check-certificate -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
ADD http://chromedriver.storage.googleapis.com/2.13/chromedriver_linux64.zip /srv/
RUN unzip /srv/chromedriver_linux64.zip -d /srv
RUN echo deb http://dl.google.com/linux/chrome/deb/ stable main >> /etc/apt/sources.list.d/google-chrome.list
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get update && apt-get install -q -y openjdk-8-jre-headless google-chrome-stable xvfb x11-xkb-utils xfonts-100dpi xfonts-75dpi xfonts-scalable xfonts-cyrillic nodejs chromium-chromedriver libgconf-2-4

RUN wget -N https://chromedriver.storage.googleapis.com/2.40/chromedriver_linux64.zip && unzip chromedriver_linux64.zip
RUN mv -f chromedriver /usr/local/share/chromedriver
RUN chmod +x /usr/local/share/chromedriver
RUN ln -s /usr/local/share/chromedriver /usr/local/bin/chromedriver && ln -s /usr/local/share/chromedriver /usr/bin/chromedriver

WORKDIR /root
RUN curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
RUN python get-pip.py
RUN pip install virtualenv && virtualenv venv
RUN . venv/bin/activate && pip install selenium sounddevice numpy

RUN mkdir -p /opt/selenium/
RUN cd /opt/selenium/ && npm install selenium-webdriver bluebird
ADD upstart.sh /opt/selenium/upstart
ADD runner.js /opt/selenium/runner.js
RUN chmod +x /opt/selenium/upstart
RUN ln -s /opt/selenium/upstart /usr/local/bin/start-test
