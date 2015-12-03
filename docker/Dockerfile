FROM ubuntu

RUN apt-get update
RUN apt-get install -y build-essential git-core libgmp3-dev graphicsmagick redis-server python-virtualenv python-dev wget

# Install nvm with node and npm
RUN git clone https://github.com/creationix/nvm.git /.nvm
RUN echo ". /.nvm/nvm.sh" >> /etc/bash.bashrc
RUN /bin/bash -c '. /.nvm/nvm.sh && nvm install v0.10.40 && nvm use v0.10.40 && nvm alias default v0.10.40 && ln -s /.nvm/v0.10.40/bin/node /usr/bin/node && ln -s /.nvm/v0.10.40/bin/npm /usr/bin/npm'

RUN npm install -g node-gyp \
 && npm cache clear

# Install Java
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update
ENV DEBIAN_FRONTEND noninteractive
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
RUN echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
RUN apt-get install -y oracle-java7-installer

# Install fxa-local-dev
RUN useradd -d /app install
RUN mkdir /app && chown install /app
USER install
RUN cd app && git clone https://github.com/mozilla/fxa-local-dev
RUN cd app/fxa-local-dev && npm install

# Run with pm2
RUN npm install -g pm2

CMD cd fxa-local-dev && pm2 start servers.json && pm2 logs

