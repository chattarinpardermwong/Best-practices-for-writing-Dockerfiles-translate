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

[คำสั่ง ENTRYPOINT อ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/#entrypoint)

การใช้งานที่เหมาะสมที่สุดสำหรับ `ENTRYPOINT` คือการ set คำสั่งหลักของ image คำสั่งนี้ยอมให้เรียกใช้ image ราวกับว่าเป็นคำสั่งนั้นๆ ได้ (จากนั้นใช้ `CMD` เป็น default flags)

เริ่มจากตัวอย่างของ image สำหรับ command line tool `s3cmd`:

```dockerfile
ENTRYPOINT ["s3cmd"]
CMD ["--help"]
```

ตอนนี้ image สามารถ run แบบนี้เพื่อแสดง command's help:

```bash
$ docker run s3cmd
```

หรือใช้ parameters ที่เหมาะสมเพื่อ run คำสั่ง:

```bash
$ docker run s3cmd ls s3://mybucket
```

นี่มีประโยชน์เพราะชื่อ image สามารถทำเพิ่มป็นการอ้างอิงไปยัง binary ที่แสดงในคำสั่งด้านบน

คำสั่ง `ENTRYPOINT` สามารถใช้ร่วมกับ script ตัวช่วยได้ ยอมให้มันทำงานในลักษณะคล้ายกับคำสั่งด้านบน แม้ว่าตอนเริ่มเครื่องมืออาจจะต้องใช้มากกว่าหนึ่งขั้นตอน

ตัวอย่างเช่น [Postgres Official Image](https://hub.docker.com/_/postgres/) ใช้ script ต่อไปนี้เป็น `ENTRYPOINT`:

```bash
#!/bin/bash
set -e

if [ "$1" = 'postgres' ]; then
    chown -R postgres "$PGDATA"

    if [ -z "$(ls -A "$PGDATA")" ]; then
        gosu postgres initdb
    fi

    exec gosu postgres "$@"
fi

exec "$@"
```

กำหนดค่า app เป็น PID 1

> script นี้ใช้ [the `exec` Bash command](http://wiki.bash-hackers.org/commands/builtin/exec) เพื่อให้ application 
> รุ่นล่าสุดที่เสถียรเป็น container's PID 1 นี้ทำให้ application สามารถรับสัญญาณ Unix ใดๆ ที่ส่งไปยัง container ได้ 
> สำหรับข้อมูลเพิ่มเติมดูได้ที่ [`ENTRYPOINT` reference](https://docs.docker.com/engine/reference/builder/#entrypoint)

script ตัวช่วยจะถูกคัดลอกลงใน container และ run ผ่าน `ENTRYPOINT` เมื่อ container เริ่มทำงาน

```dockerfile
COPY ./docker-entrypoint.sh /
ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["postgres"]
```

script นี้ให้ user interact กับ Postgres ได้หลายวิธี:

```bash
$ docker run postgres
```

หรือสามารถใช้เพื่อ run Postgres และ pass parameters ไปยัง server:

```bash
$ docker run postgres postgres --help
```

สุดท้ายก็สามารถใช้เพื่อเริ่มเครื่องมืออื่นๆ ที่แตกต่างกันอย่างสิ้นเชิง เช่น Bash:

```bash
$ docker run --rm -it postgres bash
```

### VOLUME

[คำสั่ง VOLUME อ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/#volume)

ควรใช้คำสั่ง `VOLUME` เพื่อใช้ในการเก็บ database storage area, configuration storage หรือ files/folders ใดๆ ที่สร้างโดย docker container และขอแนะนำให้ใช้ `VOLUME` สำหรับส่วน mutable หรือ/และ user-serviceable ของ image

### USER

[คำสั่ง USER อ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/#user)

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

[คำสั่ง WORKDIR อ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/#workdir)

เพื่อความชัดเจนและน่าเชื่อถือ ควรใช้ absolute paths สำหรับ `WORKDIR` เสมอ นอกจากนี้ควรใช้ `WORKDIR` แทน proliferating instructions เช่น `RUN cd … && do-something` ซึ่งยากต่อการอ่าน แก้ไข และดูแล

### ONBUILD

[คำสั่ง ONBUILD อ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/#onbuild)

คำสั่ง `ONBUILD` จะทำงานหลังจากสร้าง `Dockerfile` ปัจจุบันเสร็จสมบูรณ์ คำสั่ง `ONBUILD` จะทำงานใน child image ใดๆ `FROM` image ปัจจุบัน ให้คิดว่า `ONBUILD` เป็นคำสั่งจาก parent `Dockerfile` ไปที่ child `Dockerfile`

Docker จะใช้งานคำสั่ง `ONBUILD` ก่อนคำสั่งใดๆ ก็ตามใน child `Dockerfile`

`ONBUILD` มีประโยชน์สำหรับ images ที่กำลังจะถูก build `FROM` image ที่กำหนด ตัวอย่างเช่นจะใช้ `ONBUILD` สำหรับ language stack image ที่สร้าง arbitrary user software ที่เขียนในภาษานั้นใน `Dockerfile` อย่างที่เห็นใน [Ruby’s `ONBUILD` variants](https://github.com/docker-library/ruby/blob/c43fef8a60cea31eb9e7d960a076d633cb62ba8d/2.4/jessie/onbuild/Dockerfile)

Images ที่สร้างขึ้นด้วย `ONBUILD` ควรมี tag ที่ต่างกัน ตัวอย่างเช่น `ruby:1.9-onbuild` หรือ `ruby:2.0-onbuild`

ระวังเมื่อใส่ `ADD` หรือ `COPY` ใน `ONBUILD` ไฟล์ "onbuild" image อาจเสียหายถ้าหาก new build’s context ไม่มี resource ที่เพิ่มเข้ามา การเพิ่ม tag ที่ต่างกันตามที่แนะนำข้างต้นช่วยลดปัญหานี้ได้โดยเป็นทางเลือกให้ผู้เขียน `Dockerfile`

## ตัวอย่างสำหรับ Official Images

Official Images นี้เป็นตัวอย่างที่ดีสำหรับการสร้าง `Dockerfile`:

* [Go](https://hub.docker.com/_/golang/)
* [Perl](https://hub.docker.com/_/perl/)
* [Hy](https://hub.docker.com/_/hylang/)
* [Ruby](https://hub.docker.com/_/ruby/)

## แหล่งข้อมูลเพิ่มเติม:

* [ข้อมูลอ้างอิงจาก Dockerfile](https://docs.docker.com/engine/reference/builder/)
* [ข้อมูลเพิ่มเติมเกี่ยวกับ Base Images](https://docs.docker.com/develop/develop-images/baseimages/)
* [ข้อมูลเพิ่มเติมเกี่ยวกับ Automated Builds](https://docs.docker.com/docker-hub/builds/)
* [แนวทางการปฎิบัติสำหรับการสร้าง Official Images](https://docs.docker.com/docker-hub/official_images/)
