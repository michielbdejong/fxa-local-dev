FROM ubuntu

RUN apt-get update
RUN apt-get install -y build-essential git-core libgmp3-dev graphicsmagick redis-server python-virtualenv python-dev
RUN apt-get install wget

# Install nvm with node and npm
RUN git clone https://github.com/creationix/nvm.git /.nvm
RUN echo ". /.nvm/nvm.sh" >> /etc/bash.bashrc
RUN /bin/bash -c '. /.nvm/nvm.sh && nvm install v0.10 && nvm use v0.10 && nvm alias default v0.10 && ln -s /.nvm/v0.10/bin/node /usr/bin/node && ln -s /.nvm/v0.10/bin/npm /usr/bin/npm'

RUN npm install -g node-gyp \
 && npm cache clear

# Install fxa-local-dev
RUN git clone https://github.com/mozilla/fxa-local-dev
RUN cd fxa-local-dev && npm install
RUN cd fxa-local-dev && sh _scripts/install_all.sh

# Run with pm2
RUN npm install -g pm2
ADD ./servers.json fxa-local-dev/servers-custom.json
RUN cd fxa-local-dev && pm2 start servers-custom.json
