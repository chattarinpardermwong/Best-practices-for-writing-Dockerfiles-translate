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
