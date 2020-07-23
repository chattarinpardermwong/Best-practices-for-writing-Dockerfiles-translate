## วิธีการขียน Dockerfiles ที่ดี

ในเอกสารฉบับนี้จะอธิบายถึงวิธีการเขียนและ build Dockerfile. Docker จะสร้าง image อัตโนมัติโดยการอ่านชุดคำสั่งจาก Dockerfile ซึ่งเป็นไฟล์ที่บรรจุชุดคำสั่ง ลำดับ และ สิ่งที่ต้องใช้ในการ build image

```
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.pyogvolume01: {}
```

แต่ละชุดคำสั่งจะสร้างหนึ่งชั้น

- FROM สร้างชั้นจาก `ubuntu:18.04` สำหรับ Docker image

- COPY เพิ่มไฟล์จาก directory ปัจจุบัน

- RUN สร้าง apllication ด้วย `make`

- CMD ระบุคำสั่งที่ใช้ใน container

เมื่อ run image และ สร้าง container สามารถเพิ่มชั้นที่*สามารถเขียนได้* (container layer) การเปลี่ยนแปลงทั้งหมดที่เกี่ยวกับการ run container เช่นการเขียนไฟล์ใหม่ เปลี่ยนแปลไฟล์ที่มีอยู่แล้ว และการลบไฟล์ ทั้งหมดสามารถกำหนดได้ใน layer นี้