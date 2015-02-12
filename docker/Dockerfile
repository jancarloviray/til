# IMPORTANT: order here matters

# base image
FROM ubuntu

# author
MAINTAINER Jan Carlo Viray <virayjancarlo@yahoo.com>

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
    apparmor-utils

# Install NVM (node version manager)
RUN curl https://raw.githubusercontent.com/creationix/nvm/v0.23.3/install.sh | bash
RUN echo 'source ~/.nvm/nvm.sh' >> ~/.zshrc
RUN echo 'source ~/.nvm/nvm.sh' >> ~/.bashrc

# Install oh-my-zsh
RUN curl -L http://install.ohmyz.sh | zsh

# Install Ultimate Vim
# https://github.com/jancarloviray/spf13-vim
RUN apt-get update && apt-get install -y \
    cmake \
    python-dev \
    silversearcher-ag
RUN git clone https://github.com/jancarloviray/spf13-vim.git ~/spf13-vim
RUN /bin/bash ~/spf13-vim/bootstrap.sh
RUN cp ~/spf13-vim/.vimrc.local ~/ && cp ~/spf13-vim/.vimrc.bundles.local ~/

# install ycm dependencies (required by vim ycm plugin)
# RUN /bin/bash ~/.spf13-vim-3/.vim/bundle/YouCompleteMe/install.sh --clang-completer

# Change default shell to zsh
RUN chsh -s /usr/bin/zsh

# install vim plugins
RUN vim +BundleInstall! +BundleClean! +qall!

# tmux
RUN cp ~/spf13-vim/tmux/.tmux.conf ~/

# cleanup
# RUN rm -rf ~/spf13-vim

# fixes character and encoding issues
# http://askubuntu.com/questions/162391/how-do-i-fix-my-locale-issue
RUN apt-get install --reinstall -y language-pack-en-base
RUN sh -c "echo -e 'LANG=en_US.UTF-8\nLC_ALL=en_US.UTF-8' >> /etc/default/locale"
RUN locale-gen en_US en_US.UTF-8
RUN dpkg-reconfigure locales 
RUN update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
RUN localedef -i en_US -f UTF-8 en_US.UTF-8

# *rc files setup

# Configure Locale to UTF8
RUN echo 'export LC_ALL="en_US.UTF-8"' >> ~/.bashrc
RUN echo 'export LC_ALL="en_US.UTF-8"' >> ~/.zshrc
RUN echo 'export LANGUAGE="en_US.UTF-8"' >> ~/.bashrc
RUN echo 'export LANGUAGE="en_US.UTF-8"' >> ~/.zshrc

# fix vim colors inside tmux
RUN echo 'export TERM="xterm-256color"' >> ~/.bashrc
RUN echo 'export TERM="xterm-256color"' >> ~/.zshrc

# source: not found fix
# http://stackoverflow.com/questions/20635472/using-the-run-instruction-in-a-dockerfile-with-source-does-not-work
# RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# TODO AFTER:
# RUN source ~/.bashrc
# RUN source ~/.zshrc
# Install latest Node
# RUN nvm install stable && nvm alias default stable

# make docker image smaller
RUN apt-get clean autoclean
RUN apt-get clean autoremove -y
RUN rm -rf /var/lib{apt,dpkg,cache,log}

CMD ["zsh"]
