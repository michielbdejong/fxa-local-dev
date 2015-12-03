FROM ubuntu

ENV DEBIAN_FRONTEND noninteractive

# Install dependencies
RUN apt-get update \
 && apt-get install -y software-properties-common \
 && add-apt-repository ppa:webupd8team/java \
 && apt-get update \
 && echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
 && echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
 && apt-get install -y \
      build-essential \
      git-core \
      graphicsmagick \
      libgmp3-dev \
      oracle-java7-installer \
      python-dev \
      python-virtualenv \
      redis-server \
      wget \
 && apt-get clean

# Install nvm with node and npm
RUN git clone https://github.com/creationix/nvm.git /.nvm \
 && echo ". /.nvm/nvm.sh" >> /etc/bash.bashrc \
 && /bin/bash -c '. /.nvm/nvm.sh && nvm install v0.10.40 && nvm use v0.10.40 && nvm alias default v0.10.40 && ln -s /.nvm/v0.10.40/bin/node /usr/bin/node' \
 && /.nvm/v0.10.40/bin/npm install -g \
      node-gyp \
      npm \
      pm2 \
 && ln -s /.nvm/v0.10.40/bin/npm /usr/bin/npm \
 && npm cache clear

# Install fxa-local-dev
RUN useradd -d /app install
RUN mkdir /app && chown install /app
USER install
RUN cd app && git clone https://github.com/mozilla/fxa-local-dev
RUN cd app/fxa-local-dev && npm install
USER root

# Run with pm2
CMD cd app/fxa-local-dev && pm2 start servers.json && pm2 logs
