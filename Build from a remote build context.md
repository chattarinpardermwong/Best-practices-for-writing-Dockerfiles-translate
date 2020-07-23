### การ build จาก remote build context โดยใช้ Dockerfile จาก STDIN

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
