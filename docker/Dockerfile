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
    tmux \
    apparmor-utils

# Install NVM (node version manager)
RUN curl https://raw.githubusercontent.com/creationix/nvm/v0.23.3/install.sh | bash

# Run ~/.nvm/nvm.sh on next and every shell start
RUN echo 'source ~/.nvm/nvm.sh' >> ~/.zshrc
RUN echo 'source ~/.nvm/nvm.sh' >> ~/.bashrc

# Install oh-my-zsh
RUN curl -L http://install.ohmyz.sh | zsh

# Configure Locale to UTF8
# RUN sudo locale-gen en_US && locale-gen en_US.UTF-8
# RUN dpkg-reconfigure locales
RUN echo 'export LC_ALL="en_US.UTF-8"' >> ~/.bashrc
RUN echo 'export LC_ALL="en_US.UTF-8"' >> ~/.zshrc
RUN echo 'export LANGUAGE="en_US.UTF-8"' >> ~/.bashrc
RUN echo 'export LANGUAGE="en_US.UTF-8"' >> ~/.zshrc

# Install spf13-vim fork 
# with YCM autocomplete and tmux plugins
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

# tmux (unsupported by docker still)
# RUN cp ~/spf13-vim/tmux/.tmux.conf ~/
# RUN tmux start-server 
# RUN tmux source-file ~/.tmux.conf

# Restart shell then run nvm to install latest stable node
RUN #! /bin/bash ~/.bashrc && nvm install stable && nvm alias default stable

RUN #! /bin/bash ~/.vimrc
RUN #! /bin/bash ~/.bashrc
RUN #! /bin/bash ~/.zshrc

# Change default shell to zsh
RUN chsh -s /usr/bin/zsh

# install vim plugins
RUN vim +BundleInstall! +BundleClean! +qall!

# cleanup
# RUN rm -rf ~/spf13-vim

# make docker image smaller
RUN apt-get clean autoclean
RUN apt-get clean autoremove -y
RUN rm -rf /var/lib{apt,dpkg,cache,log}

CMD ["zsh"]
