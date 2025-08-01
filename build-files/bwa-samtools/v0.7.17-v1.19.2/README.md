# Overview
This container provides a minimal and efficient environment for running BWA and Samtools — two essential tools in next-generation sequencing (NGS) data analysis pipelines.

Built on Ubuntu:noble  with a focus on small size, security, and compatibility, this image is optimized for use in local development, CI/CD workflows, cloud environments, and cross-platform deployments (including Apple Silicon and ARM64 servers).

# Pre-installed Tools:

- BWA — Fast and sensitive read alignment for short reads

- samtools — Manipulation of SAM/BAM/CRAM files

# Essential utilities:

curl, wget, gzip, xz, tar, procps

# Quick Start

```
# Pull the latest image
docker pull zpqu/bwa-samtools:v0.7.17-v1.19.2

# Run Bowtie2 help
docker run --rm zpqu/bwa-samtools:v0.7.17-v1.19.2 bwa

# Run Samtools help
docker run --rm zpqu/bwa-samtools:v0.7.17-v1.19.2 samtools --help
```

License

- BWA: GPLv3 
- Samtools: MIT  
