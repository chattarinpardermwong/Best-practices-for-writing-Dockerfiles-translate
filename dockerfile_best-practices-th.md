# Best practices for writing Dockerfiles

<!-- TODO -->

## General guidelines and recommendations

### Create ephemeral containers

<!-- TODO -->

### Understand build context

<!-- TODO -->

### Pipe Dockerfile through `stdin`

<!-- TODO -->


#### Build an image using a Dockerfile from stdin, without sending build context

<!-- TODO -->

#### Build from a local build context, using a Dockerfile from stdin

<!-- TODO -->

#### Build from a remote build context, using a Dockerfile from stdin

<!-- TODO -->

### Exclude with .dockerignore

<!-- TODO -->

### Use multi-stage builds

<!-- TODO -->

### Don't install unnecessary packages

<!-- TODO -->

### Decouple applications

<!-- TODO -->

### Minimize the number of layers

<!-- TODO -->

### Sort multi-line arguments

<!-- TODO -->

### Leverage build cache

<!-- TODO -->

## Dockerfile instructions

<!-- TODO -->

### FROM

<!-- TODO -->

### LABEL

<!-- TODO -->

### RUN

<!-- TODO -->

#### apt-get

<!-- TODO -->

#### Using pipes

<!-- TODO -->

### CMD

<!-- TODO -->

### EXPOSE

<!-- TODO -->

### ENV

<!-- TODO -->


### ADD or COPY

<!-- TODO -->

### ENTRYPOINT

<!-- TODO -->

### VOLUME

<!-- TODO -->

### USER

<!-- TODO -->

### WORKDIR

<!-- TODO -->

### ONBUILD

<!-- TODO -->

## Examples for Official Images

These Official Images have exemplary `Dockerfile`s:

* [Go](https://hub.docker.com/_/golang/)
* [Perl](https://hub.docker.com/_/perl/)
* [Hy](https://hub.docker.com/_/hylang/)
* [Ruby](https://hub.docker.com/_/ruby/)

## Additional resources:

* [Dockerfile Reference]
* [More about Base Images]
* [More about Automated Builds]
* [Guidelines for Creating Official Images]
