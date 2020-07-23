# Best practices for writing Dockerfiles
## General guidelines and recommendations
- Create ephemeral containers
- Understand build context
- Pipe Dockerfile through `stdin`
  - Build an image using a Dockerfile from stdin, without sending build context
  - Build from a local build context, using a Dockerfile from stdin
  - Build from a remote build context, using a Dockerfile from stdin
- Exclude with .dockerignore
- [Use multi-stage builds](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Use-multi-stage-builds.md)
- [Don't install unnecessary packages](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Use-multi-stage-builds.md)
- [Decouple applications](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Decouple-applications.md)
- [Minimize the number of layers](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Minimize-the-number-of-layers.md)
- [Sort multi-line arguments](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Sort%20muiti-line-arguments.md)
- [Leverage build cache](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/Beck/Leverage%20build%20cache.md)
## Dockerfile instructions
- [FROM](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/Beck/FROM.md)
- LABEL
- RUN
- [CMD](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/chattarin/CMD.md)
- [EXPOSE](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/chattarin/EXPOSE.md)
- [ENV](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/chattarin/ENV.md)
- [ADD or COPY](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/chattarin/ADD-COPY.md)
- ENTRYPOINT
- VOLUME
- USER
- WORKDIR
- ONBUILD
## Examples for Official Images
#### These Official Images have exemplary Dockerfiles:
  - [Go](https://hub.docker.com/_/golang/)
  - [Perl](https://hub.docker.com/_/perl/)
  - [Hy](https://hub.docker.com/_/hylang/)
  - [Ruby](https://hub.docker.com/_/ruby/)
## Additional resources:
  - [Dockerfile Reference](https://docs.docker.com/engine/)
  - [More about Base Images](https://docs.docker.com/develop/develop-images/baseimages/)
  - [More about Automated Builds](https://docs.docker.com/docker-hub/builds/)
  - [Guidelines for Creating Official Images](https://docs.docker.com/docker-hub/official_images/)
