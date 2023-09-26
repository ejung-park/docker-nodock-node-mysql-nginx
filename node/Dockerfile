FROM phusion/baseimage:jammy-1.0.1

RUN apt-get update &&\
    apt-get install -y npm &&\
    npm install -g n

ARG NODE_ENV=production
ARG NODE_VERSION=latest
ARG PROJECT_PATH=/opt/app/
ARG YARN=false

ENV YARN=$YARN
ENV PROJECT_PATH=$PROJECT_PATH
ENV NODE_ENV=$NODE_ENV

#ejung
ENV NVM_DIR /usr/local/nvm 
ENV NVM_VERSION lts

RUN mkdir -p $NVM_DIR
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash

ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN echo "source $NVM_DIR/nvm.sh && \
    nvm install $NODE_VERSION && \
    nvm alias default $NODE_VERSION && \
    nvm use default" | bash



# Add
RUN groupadd -r www-app &&\
    useradd -r -g www-app www-app

RUN mkdir -p /home/www-app &&\
    chmod 777 /home/www-app -R

# Install the specified NODE_VERSION or grab latest
RUN n "$NODE_VERSION"

# Install Yarn

RUN if [ ${YARN} = true ]; then \
    npm install -g yarn \
    npm install -g nodemon \
;fi

RUN npm install -g nodemon

COPY scripts/run-nodock.sh /usr/bin/run-nodock

RUN chmod 700 /usr/bin/run-nodock

WORKDIR /opt/app

# CMD [ "nodemon", "-L", "index.js" ]