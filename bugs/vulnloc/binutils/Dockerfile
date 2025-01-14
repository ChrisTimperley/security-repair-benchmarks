FROM ubuntu:16.04

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
      bear \
      bison \
      build-essential \
      clang \
      flex \
      gcovr \
      git \
      ipython \
      libtool \
      python-dev \
      python-pip \
      texinfo \
      unzip \
      vim \
      wget \
      zip \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace
COPY build build
COPY clean clean
COPY prebuild prebuild

ARG SUT_SOURCE_PATH=/workspace/source
RUN git clone git://sourceware.org/git/binutils-gdb.git "${SUT_SOURCE_PATH}"
ARG SUT_VERSION
RUN git -C "${SUT_SOURCE_PATH}" checkout "${SUT_VERSION}"

ARG BUG_DIR
COPY "${BUG_DIR}/" ./

RUN wget -nv https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 \
 && chmod u+x jq-linux64 \
 && mv jq-linux64 /bin/jq

RUN REPAIR_TOOL=none bear ./prebuild \
 && REPAIR_TOOL=none bear ./build
