# Based on https://raw.githubusercontent.com/metabrainz/musicbrainz-docker/refs/heads/master/build/sir/Dockerfile

ARG PYTHON_VERSION=3.13
ARG BASE_IMAGE_DATE=20250313
FROM metabrainz/python:$PYTHON_VERSION-$BASE_IMAGE_DATE

ARG TARGETARCH

ARG PYTHON_VERSION
ARG BASE_IMAGE_DATE

LABEL org.metabrainz.based-on-image="metabrainz/python:${PYTHON_VERSION}-${BASE_IMAGE_DATE}"

ARG DEBIAN_FRONTEND=noninteractive

#######################
# From metabrainz/sir #
#######################

RUN set -eux \
    && apt-get update \
    && apt-get install --no-install-recommends -qy \
      ca-certificates \
      cron \
      gcc \
      git \
      libc6-dev \
      # TODO: check if this is actually needed \
      libffi-dev \
      # required for connections of pip packages \
      libssl-dev \
      # required for psycopg2. Installs without it but links against a wrong .so. \
      libpq-dev \
      # required by lxml from mb-rngpy \
      libxslt1-dev \
      libz-dev \
    && rm -rf \
      /var/lib/apt/lists/* \
      /tmp/*

##################
# Installing sir #
##################

ARG SIR_VERSION=4.0.1
LABEL org.metabrainz.sir.version="${SIR_VERSION}"

ARG DOCKERIZE_VERSION=v0.9.3
RUN curl -sSLO --retry 5 https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-$TARGETARCH-$DOCKERIZE_VERSION.tar.gz && \
    tar -C /usr/local/bin -xzvf dockerize-linux-$TARGETARCH-$DOCKERIZE_VERSION.tar.gz && \
    rm -f dockerize-linux-$TARGETARCH-$DOCKERIZE_VERSION.tar.gz

# hadolint ignore=DL3003
RUN git clone --depth=1 --branch "v${SIR_VERSION}" https://github.com/metabrainz/sir.git /code \
    && cd /code \
    && pip install -r requirements.txt \
    && rm -f /code/config.ini \
    && touch /etc/consul-template.conf

WORKDIR /code
