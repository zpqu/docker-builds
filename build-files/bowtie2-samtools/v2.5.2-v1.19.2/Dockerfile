##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####
##### Step 1. Set up the base image in the first stage.                 #####
##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####

# 'FROM' defines where the Dockerfile is starting to build from. This command has to come first in the file
# The 'as' keyword lets you name the folowing stage. The production image uses everything to the 'app' stage.

# use ubuntu:noble as base image
FROM ubuntu:noble AS app

ARG BOWTIE2VER=2.5.2
ARG SAMTOOLSVER=1.19.2

# metadata
LABEL base.image="ubuntu:noble"
LABEL dockerfile.version="1"
LABEL software="Bowtie2-samtools"
LABEL software.version="v${BOWTIE2VER}-v{SAMTOOLSVER}"
LABEL software.license="GPLv3-MIT"
LABEL description="Genome aligner Bowtie2 with samtools"
LABEL maintainer="Zhipeng Qu"

# install minimal essentials and cleanup
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        wget \
        curl \
        ca-certificates \
        xz-utils \
        tar \
        gzip \
        procps \
        bowtie2 \
        samtools && \
    apt-get autoclean && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /data

# optional: verify installations
RUN bowtie2 --version && samtools --version

# set default working directory
WORKDIR /data

# default command
CMD ["sh"]

##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####
##### Step 2. Set up the testing stage.                                 #####
##### The docker image is built to the 'test' stage before merging, but #####
##### the test stage (or any stage after 'app') will be lost.           #####
##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####

FROM app AS test

# test executables
RUN bowtie2 -h && samtools --help

# test with a toy dataset
RUN wget -q https://github.com/BenLangmead/bowtie2/raw/refs/heads/master/example/reads/reads_1.fq && \
    wget -q https://github.com/BenLangmead/bowtie2/raw/refs/heads/master/example/reads/reads_2.fq && \
  wget -q https://github.com/BenLangmead/bowtie2/raw/refs/heads/master/example/reference/lambda_virus.fa &&  \
  bowtie2-build lambda_virus.fa lambda_virus && \
  bowtie2 -x lambda_virus -1 reads_1.fq -2 reads_2.fq | samtools view -Sb - | samtools sort -o aligned.bam -
