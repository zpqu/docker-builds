# [docker-builds](#)
This repository contains the Dockerfiles and other assorted files necessary for building Docker images for bioinformatics programs. The purpose of this repository is to provide a centralized location for Docker images that is easily accessible for users, with clear documentation on how the containers were built and how to use them. The Dockerfiles follow the [template](https://github.com/StaPH-B/docker-builds/tree/master/dockerfile-template) from StaPH-B (State Public Health Lab Bioinformatics).

## Docker image repositories & hosting

- [Dockerhub - https://hub.docker.com/r/zpqu/](https://hub.docker.com/r/zpqu/)

## User Guide

You can get detailed Docker User Guide by referring to the [StaPH-B Docker User Guide](https://staphb.org/docker-builds/). And the following is a summarized usage guide for docker:

```bash
# Build a docker image to the 'test' layer
docker build --tag tool:test --target test <directory to Dockerfile>
docker build --tag bowtie2-samtools:test --target test .

# Download a docker image from dockerhub
docker pull zpqu/tool:version
docker pull zpqu/bowtie2-samtools:v2.5.2-v1.19.2

# Run the container (don't forget to mount your volumes!)
docker run --rm -u $(id -u):$(id -g) -v <local directory>:/data tool:version <command>
```

Further documentation can be found at [docs.docker.com](https://docs.docker.com/engine/reference/run/)

### Singularity support

Most Docker containers are compatible with Singularity and can easily be converted to Singularity format. Please see the [StaPH-B Docker User Guide](https://staphb.org/docker-builds/) for instructions on how to download docker images from dockerhub and how to run them using Singularity. 

### Summarized usage guide for singularity

```bash
# Pulling a container from dockerhub (creates a file)
singularity pull --name <name of singularity file> docker://zpqu/<tool>:<version>
singularity pull --name zpqu-bowtie2-samotools.simg docker://zpqu/bowtie2-samtools:v2.5.2-v1.19.2

# Running the container (don't forget to mount your volumes!)
singularity exec --bind <local directory>:/data <name of singularity file> <command>
```

Further documentation can be found at [docs.sylabs.io](https://docs.sylabs.io/guides/3.1/user-guide/cli.html)


## [Available Docker images](https://hub.docker.com/r/zpqu/)

| Software | Version | Link |
| :--------: | ------- | -------- |
| [bowtie2-samtools](https://hub.docker.com/r/zpqu/bowtie2-samtools/) <br/> [![docker pulls](https://badgen.net/docker/pulls/zpqu/bowtie2-samtools)](https://hub.docker.com/r/zpqu/bowtie2-samtools) | </li><li>[v2.5.2-v1.19.2](./bowtie2-samtools/v2.5.2-v1.19.2/)</li></ul> | https://github.com/BenLangmead/bowtie2 <br/> https://github.com/samtools/samtools |
| [bwa-samtools](https://hub.docker.com/r/zpqu/bwa-samtools/) <br/> [![docker pulls](https://badgen.net/docker/pulls/zpqu/bwa-samtools)](https://hub.docker.com/r/zpqu/bwa-samtools) | </li><li>[v0.7.17-v1.19.2](./bwa-samtools/v0.7.17-v1.19.2/)</li></ul> | https://github.com/lh3/bwa <br/> https://github.com/samtools/samtools |

You can also view the list of images on Docker hub here: https://hub.docker.com/r/zpqu/

## License
  * [GNU GPLv3 license](/LICENSE)
  * Licenses for each program are also listed as a metadata `LABEL` within each dockerfile
