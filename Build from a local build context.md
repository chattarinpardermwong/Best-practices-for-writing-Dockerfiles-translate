### การ build จาก local build context โดยใช้ Dockerfile จาก STDIN

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
