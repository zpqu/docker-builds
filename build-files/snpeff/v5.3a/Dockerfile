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

# Modify scripts to jar location
RUN for file in $(grep -iw "snpEff.jar" /snpEff/scripts/*sh | cut -f 1 -d ":" ) ; do cat $file | sed 's/snpEff.jar/\/snpEff\/snpEff.jar/g' > $file.tmp ; mv $file.tmp $file  ; done && \
  for file in $(grep -iw "SnpSift.jar" /snpEff/scripts/*sh | cut -f 1 -d ":" ) ; do cat $file | sed 's/snpEff.jar/\/snpEff\/SnpSift.jar/g' > $file.tmp ; mv $file.tmp $file  ; done && \
  chmod +x /snpEff/scripts/*sh

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

