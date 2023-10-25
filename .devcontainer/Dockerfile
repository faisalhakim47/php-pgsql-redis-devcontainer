FROM ubuntu:22.04

WORKDIR /workspace

ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ARG NODE_MAJOR=20
ARG DEBIAN_FRONTEND=noninteractiv
ARG TZ=Etc/UTC

# Install OS Dependencies
RUN apt update
RUN apt install -y --no-install-recommends sudo git ca-certificates curl gnupg software-properties-common

# Register External Repository
RUN add-apt-repository ppa:ondrej/php -y
RUN mkdir -p /etc/apt/keyrings
RUN curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
RUN echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
RUN apt update

# Install App Dependencies
RUN apt install -y --no-install-recommends nodejs php8.2-cli php8.2-mbstring php8.2-pgsql

# Install Additional Tools
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer
RUN chmod +x /usr/local/bin/composer

RUN groupadd --gid $USER_GID $USERNAME
RUN useradd --uid $USER_UID --gid $USER_GID -m $USERNAME
RUN echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME
RUN chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME

SHELL ["/bin/bash", "-lc"]

