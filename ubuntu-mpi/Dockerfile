FROM ubuntu:22.10
MAINTAINER Sameeh Jubran <sameeh.j@gmail.com>

#RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7721F63BD38B4796
RUN apt-get update && apt-get install -y autoconf automake gcc g++ make gfortran wget curl build-base libatomic gfortran git valgrind perl linux-headers openssh

RUN cd /usr/local/src/ && \
    wget http://www.mpich.org/static/downloads/3.2/mpich-3.2.tar.gz && \
    tar xf mpich-3.2.tar.gz && \
    rm mpich-3.2.tar.gz && \
    cd mpich-3.2 && \
    ./configure && \
    make && make install && \
    cd /usr/local/src && \
    rm -rf mpich-3.2

# Build Options:
# These options are passed to the steps below
ARG OPENMPI_VERSION="4.1.4"
ARG OPENMPI_MAJOR_VERSION="v4.1"
ARG OPENMPI_CONFIGURE_OPTIONS
ARG OPENMPI_MAKE_OPTIONS="-j4"

# Download, build, and install OPENMPI
RUN mkdir /tmp/openmpi-src
WORKDIR /tmp/openmpi-src
RUN wget https://download.open-mpi.org/release/open-mpi/${OPENMPI_MAJOR_VERSION}/openmpi-${OPENMPI_VERSION}.tar.gz \
      && tar xfz openmpi-${OPENMPI_VERSION}.tar.gz
RUN cd openmpi-${OPENMPI_VERSION} && ./configure ${OPENMPI_CONFIGURE_OPTIONS}
RUN cd openmpi-${OPENMPI_VERSION} && make all ${OPENMPI_MAKE_OPTIONS}
RUN cd openmpi-${OPENMPI_VERSION} && make install
RUN rm -rf /tmp/openmpi-src

#### TEST OPENMPI INSTALLATION ####
RUN mkdir /tmp/mpi-test
WORKDIR /tmp/mpi-test
COPY mpi-test .
RUN sh test.sh
RUN rm -rf /tmp/mpi-test

#### CLEAN UP ####
WORKDIR /
RUN rm -rf /tmp/*

ENV PATH=/usr/bin:/usr/local/bin:/bin:/app
