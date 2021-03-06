#
# Kurento CI Dockerfile for Ubuntu 16.04 (Xenial)
#

FROM buildpack-deps:xenial-scm

# Default to UTF-8 file.encoding
ENV LANG C.UTF-8

ENV DEBIAN_FRONTEND noninteractive

# Configure Kurento's apt proxy
RUN echo 'Acquire::http::Proxy "http://proxy.openvidu.io:3142";' > /etc/apt/apt.conf.d/01proxy \
  && echo 'Acquire::HTTP::Proxy::deb.nodesource.com "DIRECT";' >> /etc/apt/apt.conf.d/01proxy \
  && echo 'APT::Install-Recommends "false";' > /etc/apt/apt.conf.d/00recommends \
  && echo 'APT::Install-Suggests "false";' >> /etc/apt/apt.conf.d/00recommends 

#
# Install required packages
# This list is divided into 3 sections:
# 1. Miscellaneous tools that are used by several jobs in CI.
# 2. Dependencies of our build tools and packaging scripts.
#    Source: https://doc-kurento.readthedocs.io/en/latest/dev/dev_guide.html#generating-debian-packages
# 3. Extra build tools that are used by some CI scripts:
#    - jshon: used by mavenize_js_project, npm_publish.
RUN apt-get update \
  && apt-get install -y \
    gnupg \
    iproute2 \
    zip unzip \
    \
    build-essential \
    debhelper \
    curl \
    fakeroot \
    flex \
    git \
    libcommons-validator-java \
    python \
    python-apt \
    python-debian \
    python-git \
    python-requests \
    python-yaml \
    subversion \
    wget \
    \
    cmake \
    maven \
    openjdk-8-jdk \
    pkg-config \
    jshon

# Install Node.js 8.x LTS + NPM + Bower
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - \
 && apt-get install -y nodejs \
 && npm -g install bower


ENV PATH $PATH:/nodejs/bin

# Install Kurento Media Server (KMS) 
RUN echo "deb http://ubuntu.openvidu.io/dev xenial kms6" | tee /etc/apt/sources.list.d/kurento.list \
  && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5AFA7A83
  
# Cleaning the house
RUN apt-get clean && \
    apt-get autoclean && \
    rm -Rf /tmp/* && \
    rm -Rf /var/lib/apt/lists/*