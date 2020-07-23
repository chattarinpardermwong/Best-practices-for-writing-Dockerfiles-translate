### Pipe Dockerfile ผ่าน `stdin`

Docker มีความสามารถในการสร้าง image ผ่าน `stdin` ซึ่งทำให้การสร้างไม่ต้องเขัยน Dockerfile ลง disk หรือใช้ในตอนที่ `Dockerfile` สร้างแล้วไม่ต้องเก็บไฟล์ไว้

> #### ตัวอย่างต่อไปนี้คือตัวอย่างแบบง่ายแต่สามารถใช้ method ที่มากับ `stdin` ได้
> 
> ตัวอย่าง (ทั้งสองคำสั่งเหมือนกัน)
> 
> ```
> echo -e 'FROM busybox\nRUN echo "hello world"' | docker build -
> ```
> 
> ```
> docker build -<<EOF
> FROM busybox
> RUN echo "hello world"
> EOF
> ```
> 
> สามารถแทนที่ด้วยคำสั่งอะไรก้ได้

#### สร้าง image โดยใช้ Dockerfile จาก STDIN โดยไม่มี build context

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

#### การ build จาก local build context โดยใช้ Dockerfile จาก STDIN

ใช้คำสั่งในการสร้าง image ในเครื่องโดยใช้ `Dockerfile` จาก `stdin` คำสั่งที่ใช้คือ `-f` (or`--file`) เป็นตัวกำหนด `Dockerfile` ที่จะใช้ และใช้สัญลักษร์ `(-)` เพื่อบอก path ที่จะให้ Docker ไปอ่าน `Dockerfile` จาก `stdin`

```
docker build [OPTIONS] -f- PATH
```

ตัวอย่างข้างล่างแสดงให้เห็นถึง directory ที่กำลังทำงานอยู่ ถูกใช้เป็น build context เพื่อสร้าง image โดยใช้ `Dockerfile` ผ่าน `stdin` [เอกสาร](http://tldp.org/LDP/abs/html/here-docs.html)

```
# create a directory to work in
mkdir example
cd example

# create an example file
touch somefile.txt

# build an image using the current directory as context, and a Dockerfile passed through stdin
docker build -t myimage:latest -f- . <<EOF
FROM busybox
COPY somefile.txt .
RUN cat /somefile.txt
EOF
```

#### การ build จาก remote build context โดยใช้ Dockerfile จาก STDIN

ใช้คำสั่งในการสร้าง image จาก `git` โดยใช้ `Dockerfile` จาก `stdin` คำสั่งที่ใช้คือ `-f` (or`--file`) เป็นตัวกำหนด `Dockerfile` ที่จะใช้ และใช้สัญลักษร์ `(-)` เพื่อบอก path ที่จะให้ Docker ไปอ่าน `Dockerfile` จาก `stdin`

```
docker build [OPTIONS] -f- PATH
```

คำสั่งนี้มีประโยชน์เมื่อต้องการสร้าง image จาก repository ที่ไม่มี `Dockerfile` หรือต้องการสร้างด้วย `Dockerfile` ที่เขียนเอง

ตัวอย่างข้างล่างคือการสร้าง image จาก `Dockerfile` ผ่าน `stdin`และเพิ่ม `hello.c` repository ชื่อ ["hello-world" บน GitHub](https://github.com/docker-library/hello-world)

```
docker build -t myimage:latest -f- https://github.com/docker-library/hello-world.git <<EOF
FROM busybox
COPY hello.c .
EOF
```

> #### เรื่องน่ารู้
> 
> เมื่อทำการสร้าง image โดยใช้ build context จาก Git repository Docker จะทำการ `git clone` repository นั้นๆลงมาที่เครื่องคอมพิวเตอร์ และส่งไฟล์ทั้งหมดเป็น build context ให้ daemon ซึ่งนี้ทำให้ต้อง install `git` ใน host ที่จะ run คำสั่ง`docker build`
