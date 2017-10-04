FROM laradock/workspace:1.3

MAINTAINER Peter Schmalfeldt <me@peterschmalfeldt.com>

RUN groupadd -g 1000 ourstatesbot && useradd -u 1000 -g ourstatesbot -m ourstatesbot
RUN ln -snf /usr/share/zoneinfo/UTC /etc/localtime && echo UTC > /etc/timezone

COPY ./composer.json /home/ourstatesbot/.composer/composer.json

# Make sure that ~/.composer belongs to ourstatesbot
RUN chown -R ourstatesbot:ourstatesbot /home/ourstatesbot/.composer
USER ourstatesbot

# Export composer vendor path
RUN echo "" >> ~/.bashrc && echo 'export PATH="~/.composer/vendor/bin:$PATH"' >> ~/.bashrc

# Crontab
USER root

COPY ./crontab /etc/cron.d
RUN chmod -R 644 /etc/cron.d

# User Aliases

USER ourstatesbot
COPY ./aliases.sh /home/ourstatesbot/aliases.sh
RUN echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source /home/ourstatesbot/aliases.sh" >> ~/.bashrc && \
    echo "" >> ~/.bashrc
    
USER root
RUN echo "" >> ~/.bashrc && \
    echo "# Load Custom Aliases" >> ~/.bashrc && \
    echo "source /home/ourstatesbot/aliases.sh" >> ~/.bashrc && \
    echo "" >> ~/.bashrc

# xDebug:

RUN apt-get update && \
    apt-get install -y --force-yes php7.0-xdebug && \
    sed -i 's/^/;/g' /etc/php/7.0/cli/conf.d/20-xdebug.ini && \
    echo "alias phpunit='php -dzend_extension=xdebug.so /var/www/vendor/bin/phpunit'" >> ~/.bashrc

# ADD for REMOTE debugging
COPY ./xdebug.ini /etc/php/7.0/cli/conf.d/xdebug.ini
RUN sed -i 's/\[PHP\]/\[PHP\]\n\nzend_extension=\/usr\/lib\/php\/20151012\/xdebug.so/g' /etc/php/7.0/cli/php.ini

# Node / NVM:

ENV NVM_DIR /home/ourstatesbot/.nvm
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.6/install.sh | bash && \
    . /home/ourstatesbot/.nvm/nvm.sh && \
    nvm install stable && \
    nvm use stable && \
    nvm alias stable && \
    npm install -g gulp bower vue-cli

# Wouldn't execute when added to the RUN statement in the above block
# Source NVM when loading bash since ~/.profile isn't loaded on non-login shell
RUN echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc

# Add NVM binaries to root's .bashrc
USER root

RUN echo "" >> ~/.bashrc && \
    echo 'export NVM_DIR="/home/ourstatesbot/.nvm"' >> ~/.bashrc && \
    echo '[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm' >> ~/.bashrc

# YARN:

USER ourstatesbot

RUN [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" && \
    curl -o- -L https://yarnpkg.com/install.sh | bash && \
    echo "" >> ~/.bashrc && \
    echo 'export PATH="$HOME/.yarn/bin:$PATH"' >> ~/.bashrc

USER root

RUN echo "" >> ~/.bashrc && echo 'export YARN_DIR="/home/ourstatesbot/.yarn"' >> ~/.bashrc && echo 'export PATH="$YARN_DIR/bin:$PATH"' >> ~/.bashrc

USER ourstatesbot

RUN echo "" >> ~/.bashrc && echo 'export PATH="/var/www/vendor/bin:$PATH"' >> ~/.bashrc

USER root

RUN echo "" >> ~/.bashrc && echo 'alias art="php artisan"' >> ~/.bashrc

# Clean up
USER root
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Set default work directory
WORKDIR /var/www
