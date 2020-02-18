#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

FROM ubuntu:16.04

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive

# This Dockerfile adds a non-root user with sudo access. Use the "remoteUser"
# property in devcontainer.json to use it. On Linux, the container user's GID/UIDs
# will be updated to match your local UID/GID (when using the dockerFile property).
# See https://aka.ms/vscode-remote/containers/non-root-user for details.
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Configure apt, install packages and tools
RUN apt-get update \
    && apt-get -y install --no-install-recommends apt-utils dialog 2>&1 \
    #
    # Verify git, process tools, lsb-release (common in install instructions for CLIs) installed
    && apt-get -y install git iproute2 procps lsb-release 
    #

# Configure tools. 
RUN apt-get update
RUN apt-get install -y gnupg
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y curl
RUN apt-get install software-properties-common -y

# Get the Microsoft signing key
RUN curl -L https://packages.microsoft.com/keys/microsoft.asc 2>&1 | apt-key add -

# Get the Docker GPG key
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg 2>&1 | apt-key add -
# sudo apt-key adv --keyserver packages.microsoft.com --recv-keys 52E16F86FEE04B979B07E28DB02C46DF417A0893

# Azure-cli
RUN add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ xenial main"
# Add Docker source
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable"    

############### Installing Helm ###############
RUN curl -s -O https://storage.googleapis.com/kubernetes-helm/helm-v2.14.3-linux-amd64.tar.gz
RUN tar -zxvf helm-v2.14.3-linux-amd64.tar.gz
RUN mv linux-amd64/helm /usr/local/bin/helm

############### Installing kubectl ###############
RUN curl -s -LO https://storage.googleapis.com/kubernetes-release/release/v1.13.11/bin/linux/amd64/kubectl
RUN chmod +x ./kubectl
RUN mv ./kubectl /usr/local/bin/kubectl

############### Installing Packages ###############

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y apt-transport-https libssl-dev
RUN DEBIAN_FRONTEND=noninteractive apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y zip azure-cli=2.0.78-1~xenial
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y docker-ce
RUN DEBIAN_FRONTEND=noninteractive apt-get install vim -y

# Create a non-root user to use if preferred - see https://aka.ms/vscode-remote/containers/non-root-user.
RUN  groupadd --gid $USER_GID $USERNAME \
    && useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME \
    # [Optional] Add sudo support
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME  

############### Locale Settings ###############

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y locales
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Add Azure DevOps plugin for AzrureCLI
RUN az extension add --name azure-devops

ENV DOCKER_HOST tcp://host.docker.internal:2375
    
# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/* 


# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog
