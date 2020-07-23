# Best practices for writing Dockerfiles
## General guidelines and recommendations
- [Create ephemeral containers](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/develop/Create%20ephemeral%20containers.md)
- [Understand build context](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/develop/Understand%20build%20context.md)
- [Pipe Dockerfile through `stdin`](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/develop/Pipe%20Dockerfile%20through%20stdin.md)
  - [Build an image using a Dockerfile from stdin, without sending build context](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/develop/Build%20an%20image%20using%20a%20Dockerfile.md)
  - [Build from a local build context, using a Dockerfile from stdin](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/develop/Build%20from%20a%20local%20build%20context.md)
  - [Build from a remote build context, using a Dockerfile from stdin](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/develop/Build%20from%20a%20remote%20build%20context.md)
- [Exclude with .dockerignore](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/develop/exclude.md)
- [Use multi-stage builds](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Use-multi-stage-builds.md)
- [Don't install unnecessary packages](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Use-multi-stage-builds.md)
- [Decouple applications](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Decouple-applications.md)
- [Minimize the number of layers](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Minimize-the-number-of-layers.md)
- [Sort multi-line arguments](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/khan/Sort%20muiti-line-arguments.md)
- [Leverage build cache](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/Beck/Leverage%20build%20cache.md)
## Dockerfile instructions
- FROM
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
## Member  
  - Donlaporn Chantarat(Nam)      - FE
  - Jitrin Panitdirake(Beck)      - BE
  - Chattarin Pardermwong (Lego)  - BE
  - Smarch Poonkwan (Khan)        - FE
  - Araya Siriadun (Aim)          - BE
