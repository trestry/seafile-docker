FROM seafileltd/base:pi

# For suport set local time zone.
RUN export DEBIAN_FRONTEND=noninteractive && apt-get update && apt-get install tzdata -y

WORKDIR /opt/seafile

RUN mkdir -p /etc/my_init.d

ENV SEAFILE_VERSION=7.0.5 SEAFILE_SERVER=seafile-server

RUN mkdir -p /opt/seafile/ && \
    curl -sSL -o - https://github.com/haiwen/seafile-rpi/releases/download/v${SEAFILE_VERSION}/seafile-server_${SEAFILE_VERSION}_stable_pi.tar.gz \
    | tar xzf - -C /opt/seafile/

# For using TLS connection to LDAP/AD server with docker-ce.
RUN find /opt/seafile/ \( -name "liblber-*" -o -name "libldap-*" -o -name "libldap_r*" -o -name "libsasl2.so*" \) -delete

ADD scripts/create_data_links.sh /etc/my_init.d/01_create_data_links.sh

COPY scripts /scripts
COPY templates /templates

EXPOSE 80

CMD ["/sbin/my_init", "--", "/scripts/start.py"]
