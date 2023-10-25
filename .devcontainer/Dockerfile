FROM ubuntu:22.04

WORKDIR /workspace

ENV USERNAME=dev
ENV USER_UID=1000
ENV USER_GID=$USER_UID
ENV DEBIAN_FRONTEND=noninteractive
ENV NODE_MAJOR=20

# Install OS Dependencies
RUN apt update
RUN apt install -y sudo git ca-certificates curl gnupg software-properties-common

RUN groupadd --gid $USER_GID $USERNAME
RUN useradd --uid $USER_UID --gid $USER_GID -m $USERNAME
RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME
RUN chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME

SHELL ["/bin/bash", "-lc"]

# Register External Repository
RUN sudo add-apt-repository ppa:ondrej/php -y
RUN sudo mkdir -p /etc/apt/keyrings
RUN curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
RUN echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
RUN sudo apt update

# Install App Dependencies
RUN sudo apt install -y nodejs php8.2 php8.2-mbstring php8.2-pgsql

# Install Additional Tools
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN sudo mv composer.phar /usr/local/bin/composer
RUN sudo chmod +x /usr/local/bin/composer

ENV PS1="\u@\H:\w\n\$ "
