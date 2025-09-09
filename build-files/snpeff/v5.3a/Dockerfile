##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####
##### Step 1. Set up the base image in the first stage.                 #####
##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####

# 'FROM' defines where the Dockerfile is starting to build from. This command has to come first in the file
# The 'as' keyword lets you name the folowing stage. The production image uses everything to the 'app' stage.

# use ubuntu:noble as base image
FROM ubuntu:noble AS app

ARG SNPEFFVER=5.3a

# metadata
LABEL base.image="ubuntu:noble"
LABEL dockerfile.version="1"
LABEL software="snpEff"
LABEL software.version="v${SNPEFFVER}"
LABEL software.license="MIT"
LABEL description="Variant annotation tool SnpEff"
LABEL maintainer="Zhipeng Qu"

# Install OpenJDK 17 JRE and minimal tools
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        openjdk-21-jdk \
        wget \
        unzip \
        ca-certificates \
        python3 \
        curl \
    && apt-get autoclean && rm -rf /var/lib/apt/lists/*


# Download and install SnpEff v5.3a
ENV SnpEff_VERSION="5.3a"

RUN wget -q "https://snpeff.odsp.astrazeneca.com/versions/snpEff_latest_core.zip" && \
    unzip -q "snpEff_latest_core.zip" && \
    rm "snpEff_latest_core.zip"

# Modify java executables and set environment variable $PATH
RUN chmod +x /snpEff/snpEff.jar && \
  echo "#!/bin/bash" >> /snpEff/snpeff && \
  chmod +x /snpEff/SnpSift.jar && \
  echo "#!/bin/bash" >> /snpEff/snpsift && \
  echo "exec java -jar /snpEff/snpEff.jar """"$""@"""" " >> /snpEff/snpeff && \
  chmod +x /snpEff/snpeff && \
  echo "exec java -jar /snpEff/SnpSift.jar """"$""@"""" " >> /snpEff/snpsift && \
  chmod +x /snpEff/snpsift

# Fix script paths
RUN for file in $(grep -ilr "snpEff.jar" /snpEff/scripts/); do \
      sed -i 's|snpEff.jar|/snpEff/snpEff.jar|g' "$file"; \
    done && \
    for file in $(grep -ilr "SnpSift.jar" /snpEff/scripts/); do \
      sed -i 's|SnpSift.jar|/snpEff/SnpSift.jar|g' "$file"; \
    done && \
    chmod +x /snpEff/scripts/*.sh

# Create data directory and set permissions
RUN mkdir -p /snpEff/data && \
    chown -R 1001:1001 /snpEff && \
    chmod -R 755 /snpEff

# Create snpeff user
RUN groupadd --system snpeff && \
    useradd --system --create-home --home-dir /home/snpeff --shell /bin/bash --gid snpeff snpeff && \
    usermod -u 1001 snpeff && groupmod -g 1001 snpeff && \
    chown -R snpeff:snpeff /snpEff

USER snpeff

# add snpEff to PATH
ENV PATH="${PATH}:/snpEff:/snpEff/scripts"

CMD ["snpeff", "-help"]

WORKDIR /data


##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####
##### Step 2. Set up the testing stage.                                 #####
##### The docker image is built to the 'test' stage before merging, but #####
##### the test stage (or any stage after 'app') will be lost.           #####
##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####

FROM app AS test

# prints to stderr
RUN snpEff -help || snpsift -help || true

