# Prepare the image with:
#   docker build -t tensorflow/haskell:v0 docker
FROM tensorflow/tensorflow:1.0.0
MAINTAINER TensorFlow authors <tensorflow-haskell@googlegroups.com>

RUN apt-get update

RUN apt-get install -y \
        # Required by snappy-frames dependency.
        libsnappy-dev \
        # Avoids /usr/bin/ld: cannot find -ltinfo
        libncurses5-dev \
        # Makes stack viable in the container
        libgmp-dev \
        # Required for locales configuration.
        locales \
        python3-pip \
        git \
        libtinfo-dev \
        libzmq3-dev \
        libcairo2-dev \
        libpango1.0-dev \
        libmagic-dev \
        libblas-dev \
        liblapack-dev

# Our MNIST demo program outputs Unicode characters.
RUN dpkg-reconfigure locales && \
    locale-gen en_US.UTF-8 && \
    update-locale LANG=en_US.UTF-8

# Installs protoc and the libraries.
RUN \
    curl -O -L https://github.com/google/protobuf/releases/download/v3.2.0/protoc-3.2.0-linux-x86_64.zip && \
    unzip -d /usr/local protoc-3.2.0-linux-x86_64.zip bin/protoc && \
    chmod 755 /usr/local/bin/protoc && \
    curl -O https://storage.googleapis.com/tensorflow/libtensorflow/libtensorflow-cpu-linux-x86_64-1.0.0.tar.gz && \
    tar zxf libtensorflow-cpu-linux-x86_64-1.0.0.tar.gz -C /usr/local && \
    ldconfig

ENV LANG en_US.UTF-8

RUN curl -sSL https://get.haskellstack.org/ | sh

WORKDIR /

RUN git clone https://github.com/gibiansky/IHaskell

WORKDIR IHaskell

RUN pip3 install --upgrade setuptools pip && pip3 install -r requirements.txt

RUN stack install --install-ghc gtk2hs-buildtools

RUN stack install --fast

RUN stack exec ihaskell -- install --stack

RUN stack exec jupyter -- notebook
