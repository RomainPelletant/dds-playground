###### BUILD IMAGE
FROM ubuntu:22.04

# Set default shell during Docker image build to bash
SHELL ["/bin/bash", "-c"]

# Set non-interactive frontend for apt-get to skip any user confirmations
ENV DEBIAN_FRONTEND=noninteractive

# Install base packages
RUN apt-get -y update && \
	apt-get -y upgrade && \
	apt-get install --no-install-recommends -y \
		build-essential \
		ca-certificates \
		cmake \
        locales \
        libasio-dev \
		git

RUN apt-get install -y openjdk-18-jdk-headless
RUN apt-get clean

# Install multi-lib gcc (x86 only)
RUN if [ "${HOSTTYPE}" = "x86_64" ]; then \
	apt-get install --no-install-recommends -y \
		gcc-multilib \
		g++-multilib \
	; fi

# Initialise system locale
RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8

RUN git clone https://github.com/eProsima/foonathan_memory_vendor.git && \
    cd foonathan_memory_vendor && \
    mkdir build && cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release && \
    cmake --build . && \
    cmake --install . && \
    cd ../..

# Install FastDDS v3.1.0
RUN git clone --branch v3.1.0 --depth 1 https://github.com/eProsima/Fast-DDS.git && \
    cd Fast-DDS && \
    git submodule update --init --recursive && \
    # FastCDR
    cd thirdparty/fastcdr && \
    mkdir build && cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release && \
    cmake --build . && \
    cmake --install . && \
    cd ../../../ && \
    # tinyxml2
    cd thirdparty/tinyxml2 && \
    mkdir build && cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release && \
    cmake --build . && \
    cmake --install . && \
    cd ../../../ && \
    mkdir build && \
    cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release -DASIO_PATH=../thirdparty/asio/include && \
    cmake --build . && \
    cmake --install . && \
    cd ../..

# Install FastDDS-Gen
RUN cd /opt && \
    git clone --recursive https://github.com/eProsima/Fast-DDS-Gen.git fastddsgen && \
    cd fastddsgen && \
    ./gradlew assemble

ENV PATH="/opt/fastddsgen/scripts:$PATH"

# Add /usr/local/lib as default library path
RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/local.conf && ldconfig

# Create workdir folder and set as default
RUN mkdir /workdir
WORKDIR /workdir
