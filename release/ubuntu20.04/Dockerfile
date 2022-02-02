ARG ARCH=""
FROM ${ARCH}ubuntu:20.04
MAINTAINER Peter Gallagher

# Update base packages
RUN apt update \
    && apt upgrade --assume-yes

# Install pre-reqs
RUN apt install --assume-yes --no-install-recommends gnupg

# Configure Zoneminder PPA
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABE4C7F993453843F0AEB8154D0BF748776FFB04 \
    && echo deb http://ppa.launchpad.net/iconnor/zoneminder-1.36/ubuntu focal main > /etc/apt/sources.list.d/zoneminder.list \
    && apt update

# Install zoneminder
RUN DEBIAN_FRONTEND=noninteractive apt install --assume-yes zoneminder \
    && a2enconf zoneminder \
    && a2enmod rewrite cgi \
    && chown www-data. -R /usr/share/zoneminder/www

RUN ln -sf /usr/share/zoneminder/www/fonts /usr/share/zoneminder/www/skins/classic/css/fonts
RUN sed -i 's#^date.timezone.*#date.timezone = "Europe/Berlin"#' /etc/php/*/apache2/php.ini
RUN set -i 's#^ZM_PATH_ZMS.*#ZM_PATH_ZMS=/zm/cgi-bin/zms#g' /etc/zm/conf.d/*.conf
# Expose http port
EXPOSE 80

# Configure entrypoint
COPY utils/entrypoint.sh /usr/local/bin/
RUN chmod 755 /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
