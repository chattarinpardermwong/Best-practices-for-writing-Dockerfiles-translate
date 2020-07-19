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

ควรใช้คำสั่ง `VOLUME` เพื่อใช้ในการเก็บ database storage area, configuration storage หรือ files/folders ใดๆ ที่สร้างโดย docker container และขอแนะนำให้ใช้ `VOLUME` สำหรับส่วน mutable หรือ/และ user-serviceable ของ image

### USER

<!-- TODO -->

### WORKDIR

เพื่อความชัดเจนและน่าเชื่อถือ ควรใช้ absolute paths สำหรับ `WORKDIR` เสมอ นอกจากนี้ควรใช้ `WORKDIR` แทน proliferating instructions เช่น `RUN cd … && do-something` ซึ่งยากต่อการอ่าน แก้ไข และดูแล

### ONBUILD

<!-- TODO -->

## Examples for Official Images

These Official Images have exemplary `Dockerfile`s:

* [Go](https://hub.docker.com/_/golang/)
* [Perl](https://hub.docker.com/_/perl/)
* [Hy](https://hub.docker.com/_/hylang/)
* [Ruby](https://hub.docker.com/_/ruby/)

## Additional resources:

* [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
* [More about Base Images](https://docs.docker.com/develop/develop-images/baseimages/)
* [More about Automated Builds](https://docs.docker.com/docker-hub/builds/)
* [Guidelines for Creating Official Images](https://docs.docker.com/docker-hub/official_images/)
