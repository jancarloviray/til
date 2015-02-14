# TODO: this is quite an anti-pattern Dockerfile.. refactor-candidate

# base image
FROM ubuntu:14.04

# author
MAINTAINER Jan Carlo Viray

# basic requirements
RUN apt-get update \
    && apt-get install -y --fix-missing \
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
    tmux \
    ntp \
    ntpdate

# ---------------------------------------------
# TOOLS
# ---------------------------------------------

# Install oh-my-zsh
RUN curl -L http://install.ohmyz.sh | zsh

# Change default shell to zsh
RUN chsh -s /usr/bin/zsh

# Install Ultimate Vim https://github.com/jancarloviray/spf13-vim
RUN apt-get update && apt-get install -y \
    cmake \
    python-dev \
    silversearcher-ag
RUN git clone https://github.com/jancarloviray/spf13-vim.git ~/spf13-vim
RUN /bin/bash ~/spf13-vim/bootstrap.sh
RUN cp ~/spf13-vim/.vimrc.local ~/ && cp ~/spf13-vim/.vimrc.bundles.local ~/

# install vim plugins
RUN vim +BundleInstall! +BundleClean! +qall!

# tmux
RUN cp ~/spf13-vim/tmux/.tmux.conf ~/

# ---------------------------------------------
# FIXES
# ---------------------------------------------

# fixes character and encoding issues: http://askubuntu.com/questions/162391/how-do-i-fix-my-locale-issue
RUN apt-get install --reinstall -y language-pack-en-base
RUN sh -c "echo -e 'LANG=en_US.UTF-8\nLC_ALL=en_US.UTF-8' >> /etc/default/locale"
RUN locale-gen en_US en_US.UTF-8
RUN dpkg-reconfigure locales 
RUN update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
RUN localedef -i en_US -f UTF-8 en_US.UTF-8

# Configure Locale to UTF8
RUN echo 'export LC_ALL="en_US.UTF-8"' >> ~/.bashrc
RUN echo 'export LC_ALL="en_US.UTF-8"' >> ~/.zshrc
RUN echo 'export LANGUAGE="en_US.UTF-8"' >> ~/.bashrc
RUN echo 'export LANGUAGE="en_US.UTF-8"' >> ~/.zshrc

# fix vim colors inside tmux
RUN echo 'export TERM="xterm-256color"' >> ~/.bashrc
RUN echo 'export TERM="xterm-256color"' >> ~/.zshrc

# ---------------------------------------------
# SERVICES
# ---------------------------------------------

# Install NVM (node version manager)
RUN curl https://raw.githubusercontent.com/creationix/nvm/v0.23.3/install.sh | bash
RUN echo 'source ~/.nvm/nvm.sh' >> ~/.zshrc
RUN echo 'source ~/.nvm/nvm.sh' >> ~/.bashrc

# Install MongoDB
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
RUN echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
RUN apt-get update
RUN apt-get install -y mongodb-org

# Install NGINX
RUN apt-get install -y nginx

# ---------------------------------------------
# BASIC CLEANUP
# ---------------------------------------------

RUN apt-get clean autoclean
RUN apt-get clean autoremove -y
RUN rm -rf /var/lib{apt,dpkg,cache,log}

# ---------------------------------------------
# POST INSTALL
# ---------------------------------------------

# source ~/.bashrc
# source ~/.zshrc

# Install latest Node
# nvm install stable && nvm alias default stable

# Start MongoDB
# service mongod start

CMD ["zsh"]
