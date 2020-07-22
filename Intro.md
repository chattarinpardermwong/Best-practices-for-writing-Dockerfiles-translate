## วิธีการขียน Dockerfiles ที่ดี

    ในเอกสารฉบับนี้จะอธิบายถึงวิธีการเขียนและ build Dockerfile.  Docker จะสร้าง image  อัตโนมัติโดยการอ่านชุดคำสั่งจาก Dockerfile ซึ่งเป็นไฟล์ที่บรรจุชุดคำสั่ง ลำดับ และ สิ่งที่ต้องใช้ในการ build image 

```
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.pyogvolume01: {}
```

แต่ละชุดคำสั่งจะสร้างหนึ่งชั้น

    - FROM สร้างชั้นจาก  `ubuntu:18.04`  สำหรับ Docker image

    - COPY เพิ่มไฟล์จาก directory  ปัจจุบัน

    - RUN สร้าง apllication ด้วย `make`

    - CMD ระบุคำสั่งที่ใช้ใน container

เมื่อ run image และ สร้าง container สามารถเพิ่มชั้นที่*สามารถเขียนได้*  (container layer) การเปลี่ยนแปลงทั้งหมดที่เกี่ยวกับการ run container เช่นการเขียนไฟล์ใหม่ เปลี่ยนแปลไฟล์ที่มีอยู่แล้ว และการลบไฟล์ ทั้งหมดสามารถกำหนดได้ใน layer นี้

## แนวทางการปฎิบัติและคำแนะนำ

### การสร้าง ephemeral container

Image ที่สร้างมาจาก `Dockerfile` ควรสร้างให้เป็น ephemeral เท่าที่สามารถเป็นไปได้ ซึ่ง ephemeral หมายถึง container ที่สามรถหยุด ทำลายตัวเองแล้วสร้างขึ้นมาใหม่ด้วยชุดคำสั่งที่น้อยที่สุด

### ทำความเข้าใจกับการ build

เมื่อพูดถึงคำสั่ง docker build directory ที่กำลังทำงานอยู่เรียกว่า *build context*  โดยปกติ Dockerfile จะทำงานที่ build context แต่สามารถกำหนดได้เองโดยใช้ flag `(-f)` เมื่อไม่คำนึงถึงว่า Dockerfile อยู่ที่ไหน เนื้อหาของไฟล์และ directory จะถูกส่งไปที่ Docker daemon ในรูปแบบของ build content

> #### ตัวอย่างการ Build
> 
> สร้าง directoty สำหรับ build แล้ว  `cd` เพื่อเข้าไปใน directory นั้น พิมพ์  "hello" ใน text file ที่ชื่อ `hello` แล้วสร้าง Dockerfile ที่ run คำสั่ง `cat`  สร้าง image ด้วยคำสั่ง `(.)`:
> 
> ```
> mkdir myproject && cd myproject
> echo "hello" > hello
> echo -e "FROM busybox\nCOPY /hello /\nRUN cat /hello" > Dockerfile
> docker build -t helloapp:v1 .
> ```
> 
> ย้าย `Dockerfile` และ `hello` ไว้ใน directory ที่แยกกัน build image  ใช้ `-f` เพื่อเลี่ยง cache จากการ build ครั้งก่อน
> 
> ```
> mkdir -p dockerfiles context
> mv Dockerfile dockerfiles && mv hello context
> docker build --no-cache -t helloapp:v2 -f dockerfiles/Dockerfile context
> ```

การเพิ่มไฟล์ที่ไม่จำเป็นต่อการ build จะทำให้ผลลัพธ์ของการ build ใหญ่กว่า build context และทำให้ image มีขนาดใหญ่ขึ้น ทำให้ใช้เวลาในการ build pull และ push นานขึ้น สามารถดูขนาดของ build context โดยสังเกตุจากข้อความเวลาที่ buiild `Dockerfile`  ตามตัวอย่างนี้

```
Sending build context to Docker daemon  187.8MB
```
