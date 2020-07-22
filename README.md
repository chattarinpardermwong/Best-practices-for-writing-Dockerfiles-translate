# Best practices for writing Dockerfiles
## General guidelines and recommendations
- Create ephemeral containers
- Understand build context
- Pipe Dockerfile through `stdin`
  - Build an image using a Dockerfile from stdin, without sending build context
  - Build from a local build context, using a Dockerfile from stdin
  - Build from a remote build context, using a Dockerfile from stdin
- Exclude with .dockerignore
- Use multi-stage builds
- Don't install unnecessary packages
- Decouple applications
- Minimize the number of layers
- Sort multi-line arguments
- [Leverage build cache](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/Beck/Leverage%20build%20cache.md)
## Dockerfile instructions
- FROM
- LABEL
- RUN
- [CMD](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/chattarin/CMD.md)
- EXPOSE
- ENV
- [ADD or COPY](https://github.com/chattarinpardermwong/Best-practices-for-writing-Dockerfiles-translate/blob/chattarin/ADD-COPY.md)
- ENTRYPOINT
- VOLUME
- USER
- WORKDIR
- ONBUILD
## Examples for Official Images
## Additional resources:
