# base image
FROM debian

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

# Configure Locale to UTF8
# for character encoding fixes

RUN locale-gen en_US en_US.UTF8
RUN dpkg-reconfigure locales
RUN echo 'export LC_ALL="en_US.UTF-8"' >> {~/.bashrc,~/.zshrc}

# Install NVM (node version manager)
# and activate it with latest node stable
# https://github.com/creationix/nvm

RUN git clone https://github.com/creationix/nvm.git ~/.nvm \
    && cd ~/.nvm \
    && git checkout `git describe --abbrev=0 --tags` \
    && source ~/.nvm/nvm.sh \
    && source ~/.profile
RUN nvm install stable && nvm alias default stable
RUN echo 'source ~/.nvm/nvm.sh' >> {~/.bashrc,~/.zshrc}

# Change default shell to zsh

RUN chsh -s /usr/bin/zsh

# Install oh-my-zsh
# https://github.com/robbyrussell/oh-my-zsh

RUN curl -L http://install.ohmyz.sh | sh

# make docker image smaller
RUN apt-get clean autoclean
RUN apt-get clean autoremove -y
RUN rm -rf /var/lib{apt,dpkg,cache,log}

CMD ["zsh"]
