FROM ubuntu:16.04

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      autoconf \
      automake \
      autopoint \
      bear \
      bison \
      build-essential \
      clang \
      flex \
      gettext \
      git \
      gperf \
      ipython \
      libjpeg-dev \
      libtool \
      python-dev \
      python-pip \
      rsync \
      texinfo \
      unzip \
      vim \
      zip \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace

ARG SUT_SOURCE_PATH=/workspace/source

COPY build build
COPY clean clean
COPY prebuild prebuild
COPY recompile recompile

ARG BUG_DIR
COPY "${BUG_DIR}/" .

RUN unzip source.zip \
 && rm source.zip \
 && cd "${SUT_SOURCE_PATH}" \
 && autoreconf -i

RUN REPAIR_TOOL=none bear ./prebuild \
 && REPAIR_TOOL=none bear ./build

RUN cd source && make distclean