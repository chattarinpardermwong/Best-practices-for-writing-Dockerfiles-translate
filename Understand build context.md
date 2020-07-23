### ทำความเข้าใจกับการ build

เมื่อพูดถึงคำสั่ง docker build directory ที่กำลังทำงานอยู่เรียกว่า *build context* โดยปกติ Dockerfile จะทำงานที่ build context แต่สามารถกำหนดได้เองโดยใช้ flag `(-f)` เมื่อไม่คำนึงถึงว่า Dockerfile อยู่ที่ไหน เนื้อหาของไฟล์และ directory จะถูกส่งไปที่ Docker daemon ในรูปแบบของ build content

> #### ตัวอย่างการ Build
> 
> สร้าง directoty สำหรับ build แล้ว `cd` เพื่อเข้าไปใน directory นั้น พิมพ์ "hello" ใน text file ที่ชื่อ `hello` แล้วสร้าง Dockerfile ที่ run คำสั่ง `cat` สร้าง image ด้วยคำสั่ง `(.)`:
> 
> ```
> mkdir myproject && cd myproject
> echo "hello" > hello
> echo -e "FROM busybox\nCOPY /hello /\nRUN cat /hello" > Dockerfile
> docker build -t helloapp:v1 .
> ```
> 
> ย้าย `Dockerfile` และ `hello` ไว้ใน directory ที่แยกกัน build image ใช้ `-f` เพื่อเลี่ยง cache จากการ build ครั้งก่อน
> 
> ```
> mkdir -p dockerfiles context
> mv Dockerfile dockerfiles && mv hello context
> docker build --no-cache -t helloapp:v2 -f dockerfiles/Dockerfile context
> ```

การเพิ่มไฟล์ที่ไม่จำเป็นต่อการ build จะทำให้ผลลัพธ์ของการ build ใหญ่กว่า build context และทำให้ image มีขนาดใหญ่ขึ้น ทำให้ใช้เวลาในการ build pull และ push นานขึ้น สามารถดูหขานาดของ build context โดยสังเกตุจากข้อความเวลาที่ buiild `Dockerfile` ตามตัวอย่างนี้

```
Sending build context to Docker daemon  187.8MB
```
