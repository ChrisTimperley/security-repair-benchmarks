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
      zlib1g-dev \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace

COPY build build
COPY prebuild prebuild

ARG BUG_DIR
COPY "${BUG_DIR}/" .
RUN unzip source.zip \
 && rm source.zip


RUN sed -i '1137d' /workspace/source/src/main.c
RUN sed -i '1137i int main(int argc, char *argv[]) {\n' /workspace/source/src/main.c
RUN sed -i '1138i int ac=argc; char** av = argv;\n' /workspace/source/src/main.c

RUN REPAIR_TOOL=none bear ./prebuild \
 && REPAIR_TOOL=none bear ./build

