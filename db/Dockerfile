FROM postgres:9.4-alpine

ENV DEFAULT_TIMEZONE UTC

# Install some packages to use WAL
RUN apk add --no-cache \
      build-base \
      curl \
      libc6-compat \
      libffi-dev \
      linux-headers \
      python-dev \
      py-pip \
      py-cryptography \
    && pip --no-cache-dir install 'wal-e<1.0.0' envdir \
    && rm -rf /var/cache/apk/* /tmp/* /var/tmp/*

RUN apk add wget tar gcc git postgresql-dev sudo

RUN wget http://www.xunsearch.com/scws/down/scws-1.2.3.tar.bz2 \
    && tar xjf scws-1.2.3.tar.bz2 \
    && cd scws-1.2.3 \
    && ./configure \
    && make install \
    && cd .. \
    && git clone https://github.com/amutu/zhparser.git \
    && cd zhparser \
    && SCWS_HOME=/usr/local \
    && make && make install

# Add wale script
COPY setup-wale.sh /docker-entrypoint-initdb.d/

#Healthcheck to make sure container is ready
HEALTHCHECK CMD pg_isready -U $POSTGRES_USER -d $POSTGRES_DB || exit 1

# Add and configure entrypoint and command
COPY entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
CMD ["postgres"]

VOLUME ["/var/run/postgresql", "/usr/share/postgresql/", "/var/lib/postgresql/data", "/tmp", "/etc/wal-e.d/env"]
