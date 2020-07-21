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

หาก service สามารถ run without privileges ได้ ให้ใช้ `USER` เพื่อเปลี่ยนเป็น non-root user ก่อน เริ่มด้วยการสร้าง user และ group ใน `Dockerfile` ตัวอย่างเช่น `RUN groupadd -r postgres && useradd --no-log-init -r -g postgres postgres`

> UID/GID ที่ชัดเจน
>
> Users และ groups ใน image ถูกกำหนดด้วย non-deterministic UID/GID ซึ่งจะมีการกำหนดค่าถัดไปของ UID/GID โดยไม่คำนึงถึงการ 
> rebuild ของ image ดังนั้นควรกำหนด UID/GID ให้ชัดเจน

> เนื่องจาก[จุดบกพร่องที่ยังไม่ได้รับการแก้](https://github.com/golang/go/issues/13548)ใน Go archive/tar package's 
> เกี่ยวกับการการจัดการ sparse files การพยายามสร้าง user ที่มีค่า UID ที่สูงอย่างมีนัยยะสำคัญใน Docker container 
> อาจทำให้ disk exhaustion ได้ เพราะ `/var/log/faillog` ใน container layer จะเต็มไปด้วย NULL (\0) characters 
> วิธีแก้คือให้ pass `--no-log-init` flag ที่ useradd แต่ใน Debian/Ubuntu คำสั่ง `adduser` จะไม่รองรับ flag นี้

หลีกเลี่ยงการติดตั้งหรือใช้ `sudo` เนื่องจาก unpredictable TTY และ signal-forwarding behavior ที่อาจทำให้เกิดปัญหา หากต้องการฟังก์ชั่นที่คล้ายกับ `sudo` เช่นการ initialize daemon ด้วยสิทธิ์ `root` แต่ run ด้วยสิทธิ์ non-`root` ให้ไปใช้ [“gosu”](https://github.com/tianon/gosu)

สุดท้ายเพื่อลด layers และความซับซ้อน หลีกเลี่ยงการสลับ `USER` ไปมาบ่อยๆ

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
