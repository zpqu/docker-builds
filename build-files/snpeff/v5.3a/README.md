# Overview
This container provides a minimal and efficient environment for running snpEff (with bgzip and tabix support) — a popular variant annotation tool.

Built on Ubuntu:noble  with a focus on small size, security, and compatibility, this image is optimized for use in local development, CI/CD workflows, cloud environments, and cross-platform deployments (including Apple Silicon and ARM64 servers).

# Pre-installed Tools:

- snpEff — Variant annotation.

- snpSift — Annotate genomic variants using databases, filters and manipulates genomic annotated variants.

- tabix - Generic indexer for TAB-delimited genome position files

- bgzip - Block compression/decompression utility

# Essential utilities:

curl, wget, python3

# Quick Start

```
# Pull the latest image
docker pull zpqu/snpeff:v5.3a

# Run snpEff help
docker run --rm zpqu/snpeff:v5.3a snpeff

# Run snpSift help
docker run --rm zpqu/snpeff:v5.3a snpsift
```

License

- snpEff: MIT
- tabix: MIT
- bgzip: MIT
