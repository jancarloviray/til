# base image
FROM ubuntu

# author
MAINTAINER Jan Carlo Viray <virayjancarlo@yahoo.com>

# basic requirements
RUN apt-get update && apt-get install -y \
    build-essential \
    libssl-dev \
    software-properties-common \
    curl \
    wget \
    man \
    htop \
    vim \
    git \
    zsh \
    tmux

# Install NVM (node version manager)
RUN curl https://raw.githubusercontent.com/creationix/nvm/v0.23.3/install.sh | bash 

# Run ~/.nvm/nvm.sh on next and every shell start
RUN echo 'source ~/.nvm/nvm.sh' >> ~/.bashrc
RUN echo 'source ~/.nvm/nvm.sh' >> ~/.zshrc

# Install oh-my-zsh
RUN curl -L http://install.ohmyz.sh | zsh

# Configure Locale to UTF8
# RUN sudo locale-gen en_US
# RUN sudo locale-gen en_US.UTF-8
# RUN dpkg-reconfigure locales
RUN echo 'export LC_ALL="en_US.UTF-8"' >> ~/.bashrc
RUN echo 'export LC_ALL="en_US.UTF-8"' >> ~/.zshrc
RUN echo 'export LANGUAGE="en_US.UTF-8"' >> ~/.bashrc
RUN echo 'export LANGUAGE="en_US.UTF-8"' >> ~/.zshrc

# Restart shell then run nvm to install latest stable node
RUN #! /bin/bash ~/.bashrc && nvm install stable && nvm alias default stable

# Change default shell to zsh
RUN chsh -s /usr/bin/zsh

# # make docker image smaller
RUN apt-get clean autoclean
RUN apt-get clean autoremove -y
RUN rm -rf /var/lib{apt,dpkg,cache,log}

CMD ["zsh"]
