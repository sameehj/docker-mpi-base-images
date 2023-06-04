FROM ubuntu:18.04
MAINTAINER Sameeh Jubran <sameeh.j@gmail.com>

#RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7721F63BD38B4796
RUN apt-get update && apt-get install -y autoconf automake gcc g++ make gfortran wget curl gfortran git valgrind perl m4 autoconf automake flex libtool

# Build Options:
# These options are passed to the steps below
ARG OPENMPI_VERSION="4.1.4"
ARG OPENMPI_MAJOR_VERSION="v4.1"
ARG OPENMPI_CONFIGURE_OPTIONS
ARG OPENMPI_MAKE_OPTIONS="-j4"

RUN echo "nameserver 8.8.8.8" > /etc/resolv.conf
RUN echo "nameserver 192.168.20.8" >> /etc/resolv.conf
# Download, build, and install OPENMPI
RUN mkdir /tmp/openmpi-src
WORKDIR /tmp/openmpi-src
#RUN wget "https://download.open-mpi.org/release/open-mpi/v4.1/openmpi-4.1.4.tar.gz" \
#      && tar xfz openmpi-4.1.4.tar.gz
#RUN wget https://download.open-mpi.org/release/open-mpi/${OPENMPI_MAJOR_VERSION}/openmpi-${OPENMPI_VERSION}.tar.gz \
#      && tar xfz openmpi-${OPENMPI_VERSION}.tar.gz
RUN git clone https://github.com/open-mpi/ompi.git
RUN cd ompi && git checkout v4.1.4
#RUN cd openmpi-${OPENMPI_VERSION} && ./configure ${OPENMPI_CONFIGURE_OPTIONS}
#RUN cd openmpi-${OPENMPI_VERSION} && make all ${OPENMPI_MAKE_OPTIONS}
#RUN cd openmpi-${OPENMPI_VERSION} && make install
RUN cd ompi && ./autogen.pl
RUN cd ompi && ./configure ${OPENMPI_CONFIGURE_OPTIONS}
RUN cd ompi && make all ${OPENMPI_MAKE_OPTIONS}
RUN cd ompi && make install
RUN rm -rf /tmp/ompi

## FIX LD library on ubuntu
RUN ldconfig

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
