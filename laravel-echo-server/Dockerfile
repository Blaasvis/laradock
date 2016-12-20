FROM 5ndn/node:7.0

MAINTAINER Arjan Timmerman <arjan@soulfly.nl>
USER root

# Check if NVM needs to be installed
ARG NODE_VERSION=stable
ENV NODE_VERSION ${NODE_VERSION}
ARG INSTALL_NODE=false
ENV INSTALL_NODE ${INSTALL_NODE}
ENV NVM_DIR /home/laradock/.nvm
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.6/install.sh | bash && \
        . $NVM_DIR/nvm.sh && \
        nvm install ${NODE_VERSION} && \
        nvm use ${NODE_VERSION} && \
        nvm alias ${NODE_VERSION} && \
        npm install -g laravel-echo-server

USER root
# Wouldn't execute when added to the RUN statement in the above block
# Source NVM when loading bash since ~/.profile isn't loaded on non-login shell
RUN echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc

# Add NVM binaries to root's .bashrc
RUN echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="/home/laradock/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc \

ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV LARAPATH  "/root/.npm/laravel-echo-server/server.js"


CMD ["su","-","root","-c","cd /var/www && laravel-echo-server start"]


EXPOSE 6001
