FROM ruby:2.6-slim

# Give environment
ARG BUNDLE_PATH
ARG RAILS_HOME

ENV BUNDLE_PATH ${BUNDLE_PATH:-/usr/local/bundle}
ENV RAILS_HOME ${RAILS_HOME:-/home/app/rails}

RUN apt-get update -qq \
  && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
    vim-tiny \
    gcc \
    make \
    patch \
    libpq-dev \
#
# Redmine dependencies
#
# We need "gsfonts" for generating PNGs of Gantt charts
# and "ghostscript" for creating PDF thumbnails (in 4.1+)
    gsfonts \
    imagemagick \
# in 4.1+, libmagickcore-dev and libmagickwand-dev are no longer necessary/used: https://www.redmine.org/issues/30492
    libmagickcore-dev \
    libmagickwand-dev \
  && apt-get clean \
  && rm -rf /var/cache/apt/archives/* \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
  && truncate -s 0 /var/log/*log \
  #
  # To install redmine
  && mkdir -p $RAILS_HOME \
  #
  && true

WORKDIR $RAILS_HOME

EXPOSE 3000
