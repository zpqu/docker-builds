# Overview
This container provides a minimal and efficient environment for running Bowtie2 and Samtools — two essential tools in next-generation sequencing (NGS) data analysis pipelines.

Built on Ubuntu 24.04 (LTS) with a focus on small size, security, and compatibility, this image is optimized for use in local development, CI/CD workflows, cloud environments, and cross-platform deployments (including Apple Silicon and ARM64 servers).

# Pre-installed Tools:

- bowtie2 — Fast and sensitive read alignment for short reads

- samtools — Manipulation of SAM/BAM/CRAM files

#Essential utilities:

curl, wget, gzip, xz, tar, procps

# Quick Start

```
# Pull the latest image
docker pull zpqu/bowtie2-samtools:v2.5.4-v1.21

# Run Bowtie2 help
docker run --rm zpqu/bowtie2-samtools:v2.5.4-v1.21 bowtie2 --help

# Run Samtools help
docker run --rm zpqu/bowtie2-samtools:v2.5.4-v1.21 samtools --help
```

License

- Bowtie2: GPLv3 
- Samtools: MIT  

