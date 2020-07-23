### สร้าง image โดยใช้ Dockerfile จาก STDIN โดยไม่มี build context

สร้าง image ใช้ Dockerfile จาก stdin โดยไม่ใช้ไฟล์อื่นเพิ่ม `(-)` แทนตำแหน่งของ `PATH` ที่ใช้บอกให้ Docker อ่านวิธีการสร้างจาก stdin

```
docker build [OPTIONS] -
```

ตัวอย่างต่อไปนี้สร้าง image โดยใช้ Dockerfile ผ่าน stdin โดยไม่มีไฟล์ไหนส่ง build context เพิ่ม

```
docker build -t myimage:latest -<<EOF
FROM busybox
RUN echo "hello world"
EOF
```

Build context มีประโยชน์ในกรณีที่ Dockerfile ไม่ต้องการไฟล์เพื่อที่จะ copy ใส่ใน image และเพืิ่มความเร็วในการสร้าง ถ้าอยากเพิ่อมความเร็วของการสร้าง โดยเว้นบางไฟล์ไว้สามารถศึกษาเพิ่มได้จาก .dokerignore

> #### การสร้าง Dockerfile ที่ใช้ `COPY` หรือ `ADD` จะไม่สามารถใช้ตามตัวอย่างต่อไปนี้
> 
> ```
> # create a directory to work in
> mkdir example
> cd example
> 
> # create an example file
> touch somefile.txt
> 
> docker build -t myimage:latest -<<EOF
> FROM busybox
> COPY somefile.txt .
> RUN cat /somefile.txt
> EOF
> 
> # observe that the build fails
> ...
> Step 2/3 : COPY somefile.txt .
> COPY failed: stat /var/lib/docker/tmp/docker-builder249218248/somefile.txt: no such file or directory
> ```
