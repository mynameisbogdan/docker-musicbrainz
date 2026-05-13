# syntax=docker/dockerfile:1-labs

# Based on https://raw.githubusercontent.com/metabrainz/musicbrainz-docker/refs/heads/master/build/sir/Dockerfile

ARG PYTHON_VERSION=3.13
ARG BASE_IMAGE_DATE=20260216
FROM metabrainz/python:$PYTHON_VERSION-$BASE_IMAGE_DATE

ARG TARGETARCH

ARG PYTHON_VERSION
ARG BASE_IMAGE_DATE

ARG SIR_VERSION=5.0.0-rc.3
ARG DOCKERIZE_VERSION=v0.10.1

LABEL org.metabrainz.based-on-image="metabrainz/python:${PYTHON_VERSION}-${BASE_IMAGE_DATE}" \
      org.metabrainz.sir.version="${SIR_VERSION}"

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /code

ADD --checksum=931bed48c98eda808b46748d126892cff0fdc08d \
    --chown=0:0 \
    --chmod=755 \
    --exclude=.github/ \
    --exclude=docker/ \
    --exclude=docs/ \
    --exclude=test/ \
    --exclude=.dockerignore \
    --exclude=.editorconfig \
    --exclude=.gitignore \
    --exclude=config.ini \
    --exclude=requirements_dev.txt \
    --exclude=test.sh \
    --exclude=*.md \
    https://github.com/metabrainz/sir.git#v${SIR_VERSION} /code

RUN set -eux \
    && apt-get update \
    && apt-get install --no-install-recommends -qy \
      ca-certificates \
      build-essential \
    && curl -sSLO --retry 5 https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-$TARGETARCH-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-$TARGETARCH-$DOCKERIZE_VERSION.tar.gz \
    && rm -f dockerize-linux-$TARGETARCH-$DOCKERIZE_VERSION.tar.gz \
    && pip install --no-cache-dir -r requirements.txt \
    && touch /etc/consul-template.conf \
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
      build-essential \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf \
      /var/{cache,log}/* \
      /var/lib/apt/lists/* \
      /tmp/*
