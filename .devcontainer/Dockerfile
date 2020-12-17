ARG VARIANT=12
FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:${VARIANT}

ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then \
        groupmod --gid $USER_GID node \
        && usermod --uid $USER_UID --gid $USER_GID node \
        && chown -R $USER_UID:$USER_GID /home/node \
        && chown -R $USER_UID:root /usr/local/share/nvm /usr/local/share/npm-global; \
    fi

RUN apt update && export DEBIAN_FRONTEND=noninteractive \
    && apt -y install --no-install-recommends fish snapd

RUN sudo -u node npm install -g yarn pm2 lerna 

ENV POSTGRES_URI=postgresql://postgres:password@postgres
ENV MONGO_URI=mongodb://localhost
ENV NATS_URI=nats://nats:4222
ENV RABBITMQ_URI=amqp://rabbitmq:password@rabbitmq

# Install Krypt.co
RUN curl -L https://krypt.co/kr | bash

# Install OMF
RUN sudo -u node curl -L https://get.oh-my.fish | fish
RUN sudo -u node echo "omf install bobthefish" | fish

# Install Google Cloud SDK
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg  add - && apt-get update -y && apt-get install google-cloud-sdk default-jre --no-install-recommends -y

# Git Config Setup
COPY ./gitconfig /home/node/.gitconfig
RUN chmod 777 /home/node/.gitconfig

