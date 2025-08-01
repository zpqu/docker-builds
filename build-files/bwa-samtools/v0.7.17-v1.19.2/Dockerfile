##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####
##### Step 1. Set up the base image in the first stage.                 #####
##### ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- ----- #####

# 'FROM' defines where the Dockerfile is starting to build from. This command has to come first in the file
# The 'as' keyword lets you name the folowing stage. The production image uses everything to the 'app' stage.

# use ubuntu:noble as base image
FROM ubuntu:noble AS app

ARG BWA2VER=0.7.17
ARG SAMTOOLSVER=1.21

# metadata
LABEL base.image="ubuntu:noble"
LABEL dockerfile.version="1"
LABEL software="bwa-samtools"
LABEL software.version="v${BWA2VER}-v{SAMTOOLSVER}"
LABEL software.license="GPLv3-MIT"
LABEL description="Genome aligner BWA with samtools"
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
        bwa \
        samtools && \
    apt-get autoclean && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /data

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

# test with a toy dataset
RUN wget -q ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR608/003/SRR6082043/SRR6082043_1.fastq.gz -O r1.fq.gz &&\
    wget -q ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR608/003/SRR6082043/SRR6082043_2.fastq.gz -O r2.fq.gz &&\
    wget -q https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/006/945/GCF_000006945.2_ASM694v2/GCF_000006945.2_ASM694v2_genomic.fna.gz &&\
    gunzip -c GCF_000006945.2_ASM694v2_genomic.fna.gz > ref.fa

RUN bwa index ref.fa && \
    bwa mem ref.fa r1.fq.gz r2.fq.gz | \
    samtools view -Sb - | \
    samtools sort -o aligned.bam -
