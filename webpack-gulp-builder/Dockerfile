FROM node:8.10.0-alpine

ENV ALPINE_VERSION=3.6
ENV NPM_CONFIG_PREFIX=/home/node/.npm-global
# Install needed packages. Notes:
#   * dumb-init: a proper init system for containers, to reap zombie children
#   * musl: standard C library
#   * lib6-compat: compatibility libraries for glibc
#   * linux-headers: commonly needed, and an unusual package name from Alpine.
#   * build-base: used so we include the basic development packages (gcc)
#   * bash: so we can access /bin/bash
#   * git: to ease up clones of repos
#   * ca-certificates: for SSL verification during Pip and easy_install
#   * python: the binaries themselves
#   * python-dev: are used for gevent e.g.
#   * py-setuptools: required only in major version 2, installs easy_install so we can install Pip.
ENV PACKAGES="\
    dumb-init \
    musl \
    libc6-compat \
    linux-headers \
    build-base \
    bash \
    git \
    ca-certificates \
    python2 \
    python2-dev \
    py-setuptools \
"

RUN echo \
  # replacing default repositories with edge ones
    # && echo "http://dl-cdn.alpinelinux.org/alpine/v$ALPINE_VERSION/testing" > /etc/apk/repositories \
    # && echo "http://dl-cdn.alpinelinux.org/alpine/v$ALPINE_VERSION/community" >> /etc/apk/repositories \
    # && echo "http://dl-cdn.alpinelinux.org/alpine/v$ALPINE_VERSION/main" >> /etc/apk/repositories \

  # Add the packages, with a CDN-breakage fallback if needed
    && apk add --no-cache $PACKAGES || \
    (sed -i -e 's/dl-cdn/dl-4/g' /etc/apk/repositories && apk add --no-cache $PACKAGES) \

  # turn back the clock -- so hacky!
    # && echo "http://dl-cdn.alpinelinux.org/alpine/v$ALPINE_VERSION/main/" > /etc/apk/repositories \

  # && echo "@community http://dl-cdn.alpinelinux.org/alpine/v$ALPINE_VERSION/community" >> /etc/apk/repositories \
  # && echo "@testing http://dl-cdn.alpinelinux.org/alpine/v$ALPINE_VERSION/testing" >> /etc/apk/repositories \
  # && echo "@edge-main http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories \
  # make some useful symlinks that are expected to exist

    && if [[ ! -e /usr/bin/python ]];        then ln -sf /usr/bin/python2.7 /usr/bin/python; fi \
    && if [[ ! -e /usr/bin/python-config ]]; then ln -sf /usr/bin/python2.7-config /usr/bin/python-config; fi \
    && if [[ ! -e /usr/bin/easy_install ]];  then ln -sf /usr/bin/easy_install-2.7 /usr/bin/easy_install; fi \
  # Install and upgrade Pip

    && easy_install pip \
    && pip install --upgrade pip \
    && if [[ ! -e /usr/bin/pip ]]; then ln -sf /usr/bin/pip2.7 /usr/bin/pip; fi \
    && echo \
    && pip install --upgrade awscli==1.15.16

USER node
RUN mkdir /home/node/.npm-global
# Add /home/node/.npm-global/bin to path
RUN npm install -g gulp gulp-concat gulp-minify-css gulp-rename \
    gulp-sass gulp-uglify 
# webpack@3.8.1 webpack-bundle-analyzer@2.9.1

CMD ["/bin/sh"]
