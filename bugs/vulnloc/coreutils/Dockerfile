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
      gcovr \
      gettext \
      git \
      gperf \
      ipython \
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

ARG SUT_REPO_URL=https://github.com/coreutils/coreutils.git
ARG SUT_SOURCE_PATH=/workspace/source
RUN git clone "${SUT_REPO_URL}" "${SUT_SOURCE_PATH}"

COPY build build
COPY clean clean
COPY prebuild prebuild
COPY recompile recompile

# build the project-specific lib
COPY extractfix/project_specific_lib project_specific_lib
RUN cd project_specific_lib \
 && gcc -c -o hook.o hook.c \
 && ar rcs libhook.a hook.o \
 && rm hook.o

ARG SUT_VERSION
RUN cd "${SUT_SOURCE_PATH}" \
 && git checkout "${SUT_VERSION}"

ARG BUG_DIR
COPY "${BUG_DIR}/" ./

RUN REPAIR_TOOL=none bear ./prebuild \
 && REPAIR_TOOL=none bear ./build
