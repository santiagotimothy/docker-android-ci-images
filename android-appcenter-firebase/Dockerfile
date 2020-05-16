FROM openjdk:8-slim
LABEL maintainer "Timothy Santiago <santiagotimothy@gmail.com>"

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    curl \
    git \
    lib32stdc++6 \
    libglu1-mesa \
    unzip \
  && rm -rf /var/lib/apt/lists/*

# RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8

# Installing android base on what found at
# https://hub.docker.com/r/alvrme/alpine-android/~/dockerfile/

ENV SDK_TOOLS "4333796"
ENV BUILD_TOOLS "28.0.3"
ENV TARGET_SDK "28"
ENV ANDROID_HOME "/opt/sdk"

# Download and extract Android Tools
RUN curl -L http://dl.google.com/android/repository/sdk-tools-linux-${SDK_TOOLS}.zip -o /tmp/tools.zip --progress-bar && \
  mkdir -p ${ANDROID_HOME} && \
  unzip /tmp/tools.zip -d ${ANDROID_HOME} && \
  rm -v /tmp/tools.zip

# Install SDK Packages
RUN mkdir -p /root/.android/ && touch /root/.android/repositories.cfg && \
  yes | ${ANDROID_HOME}/tools/bin/sdkmanager "--licenses" && \
  ${ANDROID_HOME}/tools/bin/sdkmanager "--update" && \
  ${ANDROID_HOME}/tools/bin/sdkmanager "build-tools;${BUILD_TOOLS}" "platform-tools" "platforms;android-${TARGET_SDK}" "extras;android;m2repository" "extras;google;google_play_services" "extras;google;m2repository"

# replace shell with bash
RUN rm /bin/sh && ln -s /bin/bash /bin/sh
# nvm environment variables
ENV NVM_DIR "/opt/nvm"
ENV NODE_VERSION "10.15.3"

# install nvm
# https://github.com/creationix/nvm#install-script
RUN curl --silent -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.2/install.sh | bash

# install node and npm
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION \
    && nvm alias default $NODE_VERSION \
    && nvm use default

# add node and npm to path so the commands are available
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

# confirm installation
RUN node -v
RUN npm -v

# install appcenter
RUN npm install -g appcenter-cli

# install firebase
RUN npm install -g firebase-tools